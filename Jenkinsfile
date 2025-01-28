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
                    bat "docker build -t ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest ./docker"
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
                    ibmcloud cr login  // Log into IBM Cloud Container Registry
                    docker push ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest  // Push the image to IBM Cloud
                    """
                }
            }
        }

        // Stage to deploy the Docker image to IBM Cloud Code Engine
        stage('Deploy to IBM Cloud Code Engine') {
            steps {
                script {
                    // Deploy the built Docker image to IBM Cloud Code Engine as a new application
                    bat """
                    ibmcloud ce app create --name config-app \
                    --image ${REGISTRY}/${NAMESPACE}/${IMAGE_NAME}:latest \
                    --cpu 1 --memory 2G  // Optional: Adjust CPU and Memory as required
                    """
                }
            }
        }
    }
}
