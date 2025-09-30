pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Clona el repositorio desde GitHub
                checkout scm
            }
        }
        stage('SonarQube Analysis') {
            steps {
                // Define una variable para la herramienta SonarScanner
                // 'SonarScanner' DEBE ser el nombre exacto que pusiste en Global Tool Configuration
                def scannerHome = tool 'SonarScanner'
                
                // Usa la variable para ejecutar el scanner
                withSonarQubeEnv('sonarqube') {
                    sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=."
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

