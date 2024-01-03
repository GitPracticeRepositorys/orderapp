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
                    sh "docker image build -t ${imageName} ."
                    sh "docker image push ${imageName}"
                }
            }
        }
        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                script {
                    git branch: 'dev',
                        url: 'https://github.com/GitPracticeRepositorys/orderopsk8s.git'
                    // Use Kustomize to apply the Kubernetes configuration
                    dir("orderopsk8s/kustomize/orderopsk8s/base") {
                        sh 'ls -la'  // Debugging output
                        sh 'kubectl apply -k .'
                    }
                }
            }
        }
    }
}
