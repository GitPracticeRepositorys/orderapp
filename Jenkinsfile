pipeline {
    agent { label 'docker-node-1' }
    triggers { pollSCM('* * * * *') }
    
    environment {
        DOCKER_IMAGE_NAME = "shivakrishna99/orderapp:develop-${BUILD_NUMBER}"
        KUSTOMIZE_PATH = "/usr/local/bin/kustomize"
    }

    stages {
        stage('vcs') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/GitPracticeRepositorys/orderapp.git'
            }
        }

        stage('build and deploy') {
            steps {
                script {
                    // Build and push Docker image
                    sh "docker image build -t ${DOCKER_IMAGE_NAME} ."
                    sh "docker image push ${DOCKER_IMAGE_NAME}"
                }
            }
        }

        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                script {
                    // Clone the orderopsk8s repository
                    git branch: 'dev',
                        url: 'https://github.com/GitPracticeRepositorys/orderopsk8s.git'
                    
                    // Navigate to the overlays/dev directory
                    dir("orderopsk8s/kustomize/orderopsk8s/overlays/dev") {
                        // Edit deployment or other resources if needed
                        // For example, you can use sed or other tools to modify deployment.yaml
                        // Here, we use sed to replace a placeholder with the Docker image
                        sh "sed -i 's|<IMAGE_PLACEHOLDER>|${DOCKER_IMAGE_NAME}|' deployment.yaml"
                        
                        // Apply the modified deployment
                        sh "kubectl apply -k ."
                    }
                }
            }
        }
    }
}
