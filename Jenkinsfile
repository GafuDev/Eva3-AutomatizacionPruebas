pipeline {
    agent any
    
    environment {
        ARTIFACTORY_URL = 'https://jfrogiplacex.jfrog.io/artifactory'
        ARTIFACTORY_REPO = 'evaluacion_tres-libs-snapshot'
        ARTIFACTORY_TOKEN = 'cmVmdGtuOjAxOjE3MjQ4MTE4OTI6dHNnSVJpN0JXM3NrV2xQSldlTUxodGhENjla'
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
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.war",
                                "target": "${ARTIFACTORY_REPO}/"
                            }
                        ]
                    }"""

                    def uploadUrl = "${ARTIFACTORY_URL}/api/storage/${ARTIFACTORY_REPO}"
                    def authHeader = "Bearer ${ARTIFACTORY_TOKEN}"
                    
                    bat '''
                        @echo off
                        setlocal
                        set "CURL_COMMAND=curl -X PUT -H "Authorization: %AUTH_HEADER%" -T %UPLOAD_SPEC% %UPLOAD_URL%"
                        set "AUTH_HEADER=%ARTIFACTORY_TOKEN%"
                        set "UPLOAD_SPEC=%uploadSpec%"
                        set "UPLOAD_URL=%uploadUrl%"
                        
                        %CURL_COMMAND%
                    '''
                }
            }
        }
    }
}

