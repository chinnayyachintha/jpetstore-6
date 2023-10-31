pipeline {
    agent any
    
    environment {
        // Define environment variables as needed
        DOCKER_REPO = 'docker.io/reddy1207'
        DOCKER_PASS = "Ammu@0309"
        APP_NAME = 'jpetstore'
        DOCKER_IMAGE = "${DOCKER_REPO}/${APP_NAME}"
        DOCKER_TAG = "${DOCKER_IMAGE}:${env.BUILD_NUMBER}"
    }

    stages{
        
        stage('Clean Workspace'){
            steps{
                cleanWs()
            }
        }
        
        stage('checkout'){
            steps{
                git 'https://github.com/chinnayyachintha/jpetstore-6.git'
            }
        }
        stage('build'){
            steps{
                sh 'mvn package'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub', toolName: 'docker'){
                        // Build the Docker image
                        sh "docker build -t ${DOCKER_TAG} ."
                    }
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub', toolName: 'docker'){
                        // Push the Docker image to the repository
                        sh "docker push ${DOCKER_TAG}" 
                     }
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker-hub', toolName: 'docker'){
                        // Run the Docker container from the previously built and pushed image
                        sh "docker-compose -f  docker-compose.yaml up -d"
                    }
                }
            }
        }
    }

    post {
        always {
            // Clean up by removing the local Docker image
            script {
                sh "docker rmi -f ${DOCKER_TAG}"
            }
       }
    }
}
