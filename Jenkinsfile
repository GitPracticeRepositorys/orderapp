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
                sh "docker image build -t shivakrishna99/courses:develop-$env.BUILD_ID ."
                sh "docker image push shivakrishna99/courses:develop-$env.BUILD_ID"
            }
        }
        stage('Kustomize Deploy') {
            agent { label 'docker-node' }
            steps {
                // Use Kustomize to apply the Kubernetes configuration
                sh "cd deployments/courses/overlays/qa && kustomize edit set image courses=shivakrishna99/courses:develop-$env.BUILD_ID"
                sh 'kubectl apply -k deployments/courses/overlays/qa'
            }
        }
    }
}
