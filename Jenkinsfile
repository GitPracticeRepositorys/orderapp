pipeline {
    agent { label 'docker-node-1' }

    stages {
        stage('vcs') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/GitPracticeRepositorys/orderapp.git'
            }
        }

        stage('build and deploy') {
            steps {
                script {
                    // Build and push Docker image without explicit registry authentication
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_ID}"
                    docker.build(imageName, '.').push()
                }
            }
        }

        stage('Kustomize Deploy') {
            steps {
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd kustomize/orderopsk8s/base/kustomization.yaml && kustomize build . | kubectl apply -f -"
            }
        }
    }
}
