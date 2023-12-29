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
                sh "docker image build -t shivakrishna99/"shivakrishna99/orderapp:dev_${BUILD_ID} ."
                sh "docker image push shivakrishna99/"shivakrishna99/orderapp:dev_${BUILD_ID}"
            }
        }
        stage('Kustomize Deploy') {
            steps {
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd deployments/courses && kustomize build . | kubectl apply -f -"
            }
        }
    }
}
