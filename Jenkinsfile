void updatePomVersion(String pomVersion) {
    echo 'Updating POM version...'
    sh 'sudo cp /home/subomi/Jenkins-Test-File/pom.xml .'
    sh 'sudo chown jenkins:jenkins pom.xml'
    def pom = readMavenPom file: 'pom.xml'
    pom.version = pomVersion
    writeMavenPom model: pom
    sh 'cat pom.xml'
}

void updatePackageJsonVersion(String packageJsonVersion) {
    echo 'Updating package.json version...'
    sh 'sudo cp /home/subomi/Jenkins-Test-File/package.json .'
    sh 'sudo chown jenkins:jenkins package.json'
    sh "jq --arg newVer '${packageJsonVersion}' '.version = \$newVer' package.json > temp.json"
    sh 'mv temp.json package.json'
    sh 'cat package.json'
}

void updateProjectProperties(String assemblyVersion) {
    echo 'Updating ProjectProperties.version...'
    sh """
        sed -i.bak \
        -e 's/<AssemblyVersion>.*<\\/AssemblyVersion>/<AssemblyVersion>${assemblyVersion}<\\/AssemblyVersion>/' \
        -e 's/<FileVersion>.*<\\/FileVersion>/<FileVersion>${assemblyVersion}<\\/FileVersion>/' \
        -e 's/<VersionPrefix>.*<\\/VersionPrefix>/<VersionPrefix>${assemblyVersion}<\\/VersionPrefix>/' \
        ProjectProperties.version
    """
    sh 'cat ProjectProperties.version'
}

pipeline {
    agent any
    environment {
        POM_VERSION = "2.0.0"
        PACKAGE_JSON_VERSION = "2.0.0"
        ASSEMBLY_VERSION = "3.118"
    }
    stages {
        stage('Update All Versions') {
            steps {
                script {
                    updatePomVersion(env.POM_VERSION)
                    updatePackageJsonVersion(env.PACKAGE_JSON_VERSION)
                    updateProjectProperties(env.ASSEMBLY_VERSION)
                    sh 'realpath ProjectProperties.version'
                }
            }
        }
    }
}
