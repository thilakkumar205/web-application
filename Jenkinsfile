pipeline {
    agent any
    
    tools {
        jdk 'java-11'
        maven 'maven'
    }
    
    environment {
        IMAGE_NAME = 'project-1'
    }
    
    stages {

        stage('Git-checkout') {
            steps {
                git branch: 'dev', url: 'https://github.com/thilakkumar205/web-application.git'
            }
        }

        stage('Code Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Code Package') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'docker-hub-credentials',
                        usernameVariable: 'DOCKER_USERNAME',
                        passwordVariable: 'DOCKER_PASSWORD'
                    )]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Build and Tag Image') {
            steps {
                sh 'docker build -t $DOCKER_USERNAME/$IMAGE_NAME .'
            }
        }

        stage('Push Image') {
            steps {
                sh 'docker push $DOCKER_USERNAME/$IMAGE_NAME'
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                docker rm -f c8 || true
                docker run -d --name c8 -p 9008:8080 $DOCKER_USERNAME/$IMAGE_NAME
                '''
            }
        }
    }
}
