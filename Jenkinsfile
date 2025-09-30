pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                // Checkout explícito con credenciales para manejar submódulos
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/main']],
                    userRemoteConfigs: [[
                        url: 'https://github.com/gabrielquispe27-ops/laboratorio-3.git',
                        // Aquí se usan las credenciales
                        credentialsId: 'github-token'
                    ]],
                    extensions: [[$class: 'SubmoduleOption', recursiveSubmodules: true]]
                ])
            }
        }
        
        stage('Build') {
            steps {
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




