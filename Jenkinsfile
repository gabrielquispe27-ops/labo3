pipeline {
    agent any
    environment {
        SONARQUBE_SERVER = 'sonarqube' // Asegúrate que este es el nombre que diste en la config de Jenkins
    }
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio configurado en el pipeline
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    // Cambia 'labo3-gabriel' por una clave única para tu proyecto en SonarQube
                    sh 'sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=.'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                // Espera el resultado del Quality Gate de SonarQube
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
