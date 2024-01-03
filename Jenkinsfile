pipeline {
    agent { label 'docker-node-1' }
    triggers { pollSCM('* * * * *') }
    
    environment {
        DOCKER_IMAGE_NAME = "shivakrishna99/orderapp:develop-${BUILD_NUMBER}"
        KUSTOMIZE_PATH = "/usr/local/bin/kustomize"
    }

    stages {
        stage('VCS - Application Code') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/GitPracticeRepositorys/orderapp.git'
            }
        }

        stage('Build and Deploy - Application Code') {
            steps {
                script {
                    // Build and push Docker image
                    sh "docker image build -t ${DOCKER_IMAGE_NAME} ."
                    sh "docker image push ${DOCKER_IMAGE_NAME}"
                }
            }
        }

        stage('VCS - Kubernetes Manifests') {
            agent { label 'docker-node' }
            steps {
                git branch: 'dev',
                    url: 'https://github.com/GitPracticeRepositorys/orderappk8s.git'
            }
        }

        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                script {
                    // Use Kustomize to apply the Kubernetes configuration
                    dir("orderappk8s/kustomize/orderopsk8s/overlays/dev") {
                        sh "${KUSTOMIZE_PATH} edit set image order=${DOCKER_IMAGE_NAME}"
                        sh "kubectl apply -k ."
                    }
                }
            }
        }
    }
}
