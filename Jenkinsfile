pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/gabrielquispe27-ops/laboratorio-3.git',
                    credentialsId: 'github-token'
            }
        }
    }
}





