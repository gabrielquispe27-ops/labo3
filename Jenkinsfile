pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Para usar código Groovy, lo envolvemos en un bloque 'script'
                script {
                    // 'SonarScanner' debe ser el nombre exacto de tu herramienta en Jenkins
                    def scannerHome = tool 'SonarScanner'
                    
                    // Usamos la variable para ejecutar el scanner con su ruta completa
                    withSonarQubeEnv('sonarqube') {
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=."
                    }
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
