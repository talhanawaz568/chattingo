@Library('shared-lib') _

pipeline {
    agent any

    environment {
        DOCKERHUB_CREDS_ID = 'dockerhub-creds'
    }

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/talhanawaz568/chattingo.git'
            }
        }

        stage('Image Build') {
            steps {
                script {
                    // build backend and frontend images
                    dockerUtils.buildImage("backend", "v1")
                    dockerUtils.buildImage("frontend", "v1")
                }
            }
        }

        stage('Filesystem Scan') {
            steps {
                sh 'echo "Running static code analysis..."'
                // Add tools like SonarQube or Trivy if needed
            }
        }

        stage('Image Scan') {
            steps {
                sh 'echo "Scanning Docker images for vulnerabilities..."'
                // Example: trivy image talha884/backend:v1
            }
        }

        stage('Push to Registry') {
            steps {
                script {
                    docker.withRegistry('', DOCKERHUB_CREDS_ID) {
                        dockerUtils.pushImage("backend", "v1")
                        dockerUtils.pushImage("frontend", "v1")
                    }
                }
            }
        }

        stage('Update Compose') {
            steps {
                sh '''
                sed -i 's|image: talha884/backend:.*|image: talha884/backend:v1|' docker-compose.yml
                sed -i 's|image: talha884/frontend:.*|image: talha884/frontend:v1|' docker-compose.yml
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                docker-compose up 
                '''
            }
        }
    }
}
