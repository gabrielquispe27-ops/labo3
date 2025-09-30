pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build') {
            steps {
                // Entra en la carpeta del proyecto antes de compilar
                dir('primer-api-rest') {
                    script {
                        def mvnHome = tool 'Maven_3.9.6'
                        sh "${mvnHome}/bin/mvn clean install -DskipTests"
                    }
                }
            }
        }

        stage('SonarQube Analysis') {
            steps {
                // Entra también en la carpeta para el análisis
                dir('primer-api-rest') {
                    script {
                        def scannerHome = tool 'SonarScanner'
                        withSonarQubeEnv('sonarqube') {
                            sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=. -Dsonar.java.binaries=target/classes"
                        }
                    }
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


