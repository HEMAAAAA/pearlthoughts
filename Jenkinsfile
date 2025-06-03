   pipeline {
    agent any
    
    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub')
        DOCKERHUB_REPO = 'hema995' // Change this to your DockerHub username
    }
    
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        
        stage('Build & Push Images') {
            parallel {
                stage('Support Service') {
                    steps {
                        dir('contact-support-team') {
                            buildAndPushDockerImage('contact-support-team')
                        }
                    }
                }
                
                stage('User Service') {
                    steps {
                        dir('profile-management') {
                            buildAndPushDockerImage('profile-management')
                        }
                    }
                }
                
                stage('Order Service') {
                    steps {
                        dir('order-management') {
                            buildAndPushDockerImage('order-management')
                        }
                    }
                }
                
                stage('Payment Service') {
                    steps {
                        dir('shipping-and-handling') {
                            buildAndPushDockerImage('shipping-and-handling')
                        }
                    }
                }
                
                stage('Inventory Service') {
                    steps {
                        dir('product-inventory') {
                            buildAndPushDockerImage('product-inventory')
                        }
                    }
                }
                
                stage('frontend') {
                    steps {
                        dir('ecommerce-ui') {
                            buildAndPushDockerImage('ecommerce-ui')
                        }
                    }
                }
                
                stage('catalog-service') {
                    steps {
                        dir('product-catalog') {
                            buildAndPushDockerImage('product-catalog')
                        }
                    }
                    
                }
                // Add more stages for other services as needed
            }
        }
    }
    
    post {
        always {
            cleanWs()
        }
        success {
            echo 'All Docker images built and pushed successfully!'
        }
        failure {
            echo 'Pipeline failed. Check the logs for details.'
        }
    }
}

def buildAndPushDockerImage(String serviceName) {
    script {
         // Login to DockerHub
        withCredentials([usernamePassword(credentialsId: 'dockerhub', passwordVariable: 'DOCKERHUB_PASSWORD', usernameVariable: 'DOCKERHUB_USERNAME')]) {
            sh "docker login -u ${env.DOCKERHUB_USERNAME} -p ${env.DOCKERHUB_PASSWORD}"
        // Build the Docker image
        docker.build("${env.DOCKERHUB_REPO}/${serviceName}:${env.BUILD_NUMBER}")
        }
        
        // Push the Docker image
        docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
            docker.image("${env.DOCKERHUB_REPO}/${serviceName}:${env.BUILD_NUMBER}").push()
            
            // Optionally push as 'latest'
            docker.image("${env.DOCKERHUB_REPO}/${serviceName}:${env.BUILD_NUMBER}").push('latest')
        }
        
        echo "Successfully pushed ${env.DOCKERHUB_REPO}/${serviceName}:${env.BUILD_NUMBER} to DockerHub"
    }
}
