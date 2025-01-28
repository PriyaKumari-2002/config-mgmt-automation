pipeline {
    agent any
    environment {
        REGISTRY = "jp.icr.io"                 // IBM Cloud Container Registry URL
        NAMESPACE = "config-manage"            // Your IBM Cloud Container Registry namespace
        IMAGE_NAME = "app1-image"              // Your Docker image name
        IBM_CLI_API_KEY = credentials('ibmcloud-api-key')  // IBM Cloud API key for authentication
    }
    stages {
        // Stage to checkout the code from GitHub or your Git repository
        stage('Checkout') {
            steps {
                checkout scm  // Checkout the source code from Git repository
            }
        }

        // Stage to build the Docker image
        stage('Build Docker Image') {
            steps {
                script {
                    // Build the Docker image locally using the Dockerfile in your repository
                    bat "docker build -f Docker/app1/Dockerfile -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest ."
                    bat "docker build -f Docker/app2/Dockerfile -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest ."
                }
            }
        }

        // Stage to push the Docker image to IBM Cloud Container Registry
        stage('Push to IBM Cloud Container Registry') {
            steps {
                script {
                    // Log in to IBM Cloud using the API key, set the region to 'jp' for Japan
                bat """
                  ibmcloud login --apikey ${IBM_CLI_API_KEY} -r jp
                  ibmcloud cr login
                  docker push ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest
                """

                }
            }
        }
    }
}

