pipeline {
    agent any

    stages {
        stage('Git-checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/thilakkumar205/web-application.git'
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

        stage('Build and tag') {
            steps {
                sh 'docker build -t thilakkumar205/project-1 .'
            }
        }

        stage('Containerisation') {
            steps {
                sh '''
                docker rm -f c1812101 || true
                docker run -d --name c1812101 -p 9002:8080 thilakkumar205/project-1
                '''
            }
        }

        stage('Login to Docker Hub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh "echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin"
                    }
                }
            }
        }

        stage('Pushing image to repository') {
            steps {
                sh 'docker push thilakkumar205/project-1'
            }
        }
    }
}
