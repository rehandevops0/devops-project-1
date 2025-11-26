pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = 'dockerhub-creds' //  Jenkins DockerHub credentials ID
        IMAGE_NAME = 'rehandevops/webapp-cicd'   //  DockerHub username
        KUBE_CONFIG = '/var/lib/jenkins/.kube/config' // Path to kubeconfig if needed
    }

    stages {
        stage('Checkout') {
            steps {
                // Explicitly checkout main branch
                git branch: 'main', 
                    url: 'https://github.com/rehandevops0/devops-project-1.git', 
                    credentialsId: "${DOCKERHUB_CREDENTIALS}"
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh "docker build -t ${IMAGE_NAME}:latest ."
                }
            }
        }

        stage('Login & Push to DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: "${DOCKERHUB_CREDENTIALS}", 
                                                     usernameVariable: 'DOCKER_USER', 
                                                     passwordVariable: 'DOCKER_PASS')]) {
                        sh "echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin"
                        sh "docker push ${IMAGE_NAME}:latest"
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                script {
                    // Apply Kubernetes manifests (replace with your deployment YAML)
                    sh "kubectl --kubeconfig=${KUBE_CONFIG} apply -f k8s/deployment.yaml"
                    sh "kubectl --kubeconfig=${KUBE_CONFIG} apply -f k8s/service.yaml"
                }
            }
        }
    }

    post {
        always {
            echo 'Cleaning up workspace...'
            cleanWs()
        }
        success {
            echo 'Pipeline completed successfully!'
        }
        failure {
            echo 'Pipeline failed.'
        }
    }
}

