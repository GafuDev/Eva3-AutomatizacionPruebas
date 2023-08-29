pipeline {
    agent any
    
    stages {
        stage('Checkout') {
            steps {
                // Clona repositorio
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Compila proyecto
                bat 'mvn clean install'
            }
        }
        
        stage('Generate WAR') {
            steps {
                // Genera archivo .WAR
                bat 'mvn package'
            }
        }

        stage('Publish to Artifactory') {
            steps {
                script {
                    // Crear el servidor Artifactory
                    def server = Artifactory.server(
                        id: 'evaluacionTres',
                        url: 'https://jfrogiplacex.jfrog.io/artifactory/api/maven/evaluacion_tres-libs-snapshot',
                        credentialsId: 'jenkins'
                    )
                    
                    // Publicar el archivo .WAR en Artifactory
                    def uploadSpec = """{
                        "files": [
                            {
                                "pattern": "target/*.war",
                                "target": "evaluacion_tres-libs-snapshot/"
                            }
                        ]
                    }"""
                    server.upload(uploadSpec)
                }
            }
        }
    }
}

