pipeline {
    agent any
    
    environment {
        DOCKER_IMAGE = 'test-angular-app2'
    }

    stages {
        stage('Checkout Code') {
            steps {
                // Checkout the Angular application source code from your SCM (e.g., Git)
                checkout scm
            }
        }
        
        stage('Build Angular App') {
            steps {
                // Install dependencies and build the Angular app
                script {
                    sh 'npm install'
                    sh 'ng build --prod'
                }
            }
        }
        
        stage('Build Docker Image') {
            steps {
                // Build a Docker image from the Dockerfile in the project directory
                script {
                    sh "docker build -t ${DOCKER_IMAGE}:$BUILD_NUMBER ."
                }
            }
        }
        
        stage('Push Docker Image') {
            steps {
                // Push the Docker image to a registry (e.g., Docker Hub)
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerHubCredentials', usernameVariable: 'USERNAME', passwordVariable: 'PASSWORD')]) {
                        sh "docker login -u $USERNAME -p $PASSWORD"
                        sh "docker push ${DOCKER_IMAGE}:$BUILD_NUMBER"
                    }
                }
            }
        }
    }
}
