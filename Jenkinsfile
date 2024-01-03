pipeline {
    agent { label 'docker-node-1' }
    triggers { pollSCM('* * * * *') }
    stages {
        stage('vcs') {
            steps {
                git branch: 'develop',
                    url: 'https://github.com/GitPracticeRepositorys/StudentCoursesRestAPI.git'
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
                sh "cd orderopsk8s/kustomize/orderopsk8s/overlays/dev && kustomize edit set image courses=shivakrishna99/courses:develop-$env.BUILD_ID"
                sh 'kubectl apply -k kustomize/orderopsk8s/overlays/dev'
            }
        }
    }
}
