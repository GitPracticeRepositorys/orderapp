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
                    
                    // Use Kustomize to apply the Kubernetes configuration
                    dir("orderopsk8s/kustomize/orderopsk8s/base") {
                        sh 'ls -la'  // Debugging output
                        sh "${KUSTOMIZE_PATH} edit set image order=${DOCKER_IMAGE_NAME}"
                        sh "kubectl apply -k overlays/dev"
                    }
                }
            }
        }
    }
}
