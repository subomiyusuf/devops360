void updateVersion(String filePath, String newVersion, String pattern) {
    sh """
        sed -i.bak "s/${pattern}.*<\\/${pattern}>${pattern}${newVersion}<\\/${pattern}/g" ${filePath}
        [ $? -eq 0 ] || exit 1
    """
}

pipeline {
    agent any
    
    environment {
        VERSION_DIR = credentials('version-dir')
        VERSIONS = '''
            POM_VERSION=2.0.0
            PACKAGE_JSON_VERSION=2.0.0
            ASSEMBLY_VERSION=3.118
        '''
    }
    
    stages {
        stage('Update Versions') {
            steps {
                script {
                    sh '''
                        set -e
                        
                        # POM
                        cp ${VERSION_DIR}/pom.xml . && \
                        sed -i "s/<version>.*<\/version>/<version>2.0.0<\/version>/" pom.xml
                        
                        # package.json
                        cp ${VERSION_DIR}/package.json . && \
                        jq '.version = "2.0.0"' package.json > temp.json && \
                        mv temp.json package.json
                        
                        # ProjectProperties
                        sed -i "s/<AssemblyVersion>.*<\/AssemblyVersion>/<AssemblyVersion>3.118<\/AssemblyVersion>/" ProjectProperties.version
                    '''
                }
            }
        }
    }
}
