pipeline {
    agent any
    
    environment {
        ARTIFACTORY_URL = 'https://jfrogiplacex.jfrog.io/artifactory'
        ARTIFACTORY_REPO = 'evaluacion_tres-libs-snapshot'
        ARTIFACTORY_TOKEN = 'cmVmdGtuOjAxOjE3MjQ4MTI3NTQ6VWhKelo5Yjl5UHNmYzh1UEh4V1Y3M2Fqc3BN'
    }
    
    stages {
        stage('Checkout') {
            steps {
                // Clonar repositorio
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Compilar proyecto
                bat 'mvn clean install'
            }
        }
        
        stage('Generate WAR') {
            steps {
                // Generar archivo .WAR
                bat 'mvn package'
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    def authHeader = "Authorization: Bearer ${ARTIFACTORY_TOKEN}"
                    def uploadSpec = '{ "files": [ { "pattern": "target/*.war", "target": "' + ARTIFACTORY_REPO + '/" } ] }'
                    def uploadUrl = "${ARTIFACTORY_URL}/${ARTIFACTORY_REPO}"
                    
                    bat """
                        echo ${uploadSpec} > uploadSpec.json
                        curl -X PUT -H "${authHeader}" -H "Content-Type: application/json" -d @uploadSpec.json "${uploadUrl}"
                    """
                }
            }
        }
    }
}


