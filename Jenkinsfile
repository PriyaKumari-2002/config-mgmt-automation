pipeline {
    agent any
    environment {
        REGISTRY = "jp.icr.io"                 // IBM Cloud Container Registry URL
        NAMESPACE = "config-manage"            // Your IBM Cloud Container Registry namespace
        IMAGE_NAME_APP1 = "app1-image"         // Docker image name for app1
        IMAGE_NAME_APP2 = "app2-image"         // Docker image name for app2
        IBM_CLI_API_KEY = credentials('ibmcloud-api-key')  // IBM Cloud API key for authentication
    }
    stages {
        // Stage to checkout the code from GitHub or your Git repository
        stage('Checkout') {
            steps {
                checkout scm  // Checkout the source code from Git repository
            }
        }

        // Stage to build the Docker images for app1 and app2
        stage('Build Docker Images') {
            steps {
                script {
                    // Build Docker image for app1
                    bat "docker build -f Docker/app1/Dockerfile -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME_APP1}:latest ."
                    // Build Docker image for app2
                    bat "docker build -f Docker/app2/Dockerfile -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME_APP2}:latest ."
                }
            }
        }

        // Stage to push the Docker images to IBM Cloud Container Registry
        stage('Push to IBM Cloud Container Registry') {
            steps {
                script {
                    // Log in to IBM Cloud using the API key, set the region to 'jp' for Japan
                    bat """
                      ibmcloud login --apikey ${IBM_CLI_API_KEY} -r jp
                      ibmcloud cr login
                      docker push ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME_APP1}:latest
                      docker push ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME_APP2}:latest
                    """
                }
            }
        }
    }
}
