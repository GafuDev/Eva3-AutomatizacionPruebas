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
    }
}
