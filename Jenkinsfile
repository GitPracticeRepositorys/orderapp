pipeline {
    agent { label 'docker-node-1' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                git branch: 'dev',
                    url: 'https://github.com/GitPracticeRepositorys/orderapp.git'
            }
        }
        stage('build and deploy') {
            steps {
                sh "docker image build -t shivakrishna99/orderapp:dev_${BUILD_NUMBER} ."
                sh "docker image push shivakrishna99/orderapp:dev_${BUILD_NUMBER}"
            }
        }
        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd orderopsk8s/manifests"
                sh 'kubectl apply -f orderdeploy.yaml'
                sh ' kubectl apply -f ordersvc.yaml
            }
        }
    }
}
