pipeline {
    agent any

    environment {
        orderappRepo = 'https://github.com/GitPracticeRepositorys/orderapp.git'
        kustomizeRepo = 'https://github.com/GitPracticeRepositorys/kustomize-config.git'
        kustomizePath = '/usr/local/bin/kustomize'
    }

    stages {
        stage('VCS - Order App') {
            steps {
                // Checkout the Git repository for the main application
                git branch: 'main', url: orderappRepo
            }
        }

        stage('VCS - Kustomize Config') {
            steps {
                // Checkout the Git repository for Kustomize configurations
                script {
                    dir('kustomize-config') {
                        git branch: 'main', url: kustomizeRepo
                    }
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    // Build and push Docker image
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_ID}"
                    docker.build(imageName, '.').push()
                }
            }
        }

        stage('Kustomize Deploy') {
            steps {
                script {
                    // Use Kustomize to apply the Kubernetes configuration
                    sh "cd kustomize-config/base && $kustomizePath build . | kubectl apply -f -"
                }
            }
        }
    }

    post {
        always {
            // Clean up steps, if needed
        }
    }
}
