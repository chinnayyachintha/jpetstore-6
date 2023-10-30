pipeline {
    agent {label 'Arun'}
    
    environment {
        // Define environment variables as needed
        DOCKER_REPO = 'docker.io/arun9182'
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
                checkout scmGit(branches: [[name: '*/master']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/arun1267/jpetstore-6.git']])
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
                    // Build the Docker image
                    sh "docker build -t ${DOCKER_TAG} ."
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    // Push the Docker image to the repository
                    sh "docker push ${DOCKER_TAG}"
                }
            }
        }
        
        stage('Run Docker Container') {
            steps {
                script {
                    // Run the Docker container from the previously built and pushed image
                    sh "docker run -d --name ${APP_NAME} -p 8085:8080 ${DOCKER_TAG}"
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
