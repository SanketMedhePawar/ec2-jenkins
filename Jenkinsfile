pipeline {
    agent any

    tools {
        maven 'MyMaven'  // Make sure this matches exactly the name in Jenkins config
    }
    
    environment {
        IMAGE_NAME = "sanketmedhepawar/jenkins-docker-maven"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/SanketMedhePawar/ec2-jenkins.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Print App Output (Local JAR Run)') {
            steps {
                sh 'java -jar target/jenkins-docker-maven-1.0-SNAPSHOT.jar'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${IMAGE_NAME}:latest")
                }
            }
        }

        stage('Print App Output (Docker Run)') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:latest"
            }
        }

        stage('Push to DockerHub') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    script {
                        docker.withRegistry('https://index.docker.io/v1/', 'dockerhub-creds') {
                            dockerImage.push()
                        }
                    }
                }
            }
        }
    }
}
