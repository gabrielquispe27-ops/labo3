pipeline {
    agent any

    // AÑADE ESTE BLOQUE
    tools {
        // Asegúrate que 'SonarScanner' es el nombre exacto que le diste
        // en 'Manage Jenkins' > 'Global Tool Configuration'
        tool 'SonarScanner'
    }

    environment {
        SONARQUBE_SERVER = 'sonarqube'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    // Ahora este comando funcionará
                    sh 'sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=.'
                }
            }
        }
        stage('Quality Gate') {
            steps {
                timeout(time: 5, unit: 'MINUTES') {
                    waitForQualityGate abortPipeline: true
                }
            }
        }
    }
}
