pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        // --- NUEVO STAGE: COMPILAR EL PROYECTO ---
        stage('Build') {
            steps {
                // Este comando compila tu proyecto Java usando Maven.
                // Si tu proyecto usa Gradle, el comando sería: sh './gradlew build'
                sh 'mvn clean install -DskipTests'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                script {
                    def scannerHome = tool 'SonarScanner'
                    withSonarQubeEnv('sonarqube') {
                        // Se añade la propiedad 'sonar.java.binaries'
                        sh "${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=labo3-gabriel -Dsonar.sources=. -Dsonar.java.binaries=target/classes"
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
