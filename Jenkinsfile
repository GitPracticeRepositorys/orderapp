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
                script {
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_NUMBER}"
                    def orderopsk8sDir = env.HOME + "/orderopsk8s"
                    def kustomizepath = "/usr/local/bin/kustomize"
                    sh "docker image build -t shivakrishna99/orderapp:dev_${BUILD_NUMBER} ."
                    sh "docker image push shivakrishna99/orderapp:dev_${BUILD_NUMBER}"
                }
            }
        }
        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                git branch: 'dev',
                    url: 'https://github.com/GitPracticeRepositorys/orderopsk8s.git'
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd orderopsk8s/kustomize/orderopsk8s/base"
                sh 'kubectl apply -k .'
            }
        }
    }
}
