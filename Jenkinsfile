pipeline {
    agent any

    stages {
        stage('dev') {
            steps {
                echo 'I am in Dev'
                sh 'git --version'
            }
        }

        stage('pro') {
            steps {
                echo 'I am in Prod'
                sh 'docker --version'
            }
        }
    }
}

