pipeline {
    agent any 

    stages {
        stage('checkout') {
            steps {
                git url: 'https://github.com/GitPracticeRepo/orderapp.git',
                    branch: "main"
            }
            
        }
        stage('build and push the image ') {
            steps {
                sh "docker image build -t shaikkhajaibrahim/orderapp:dev_${BUILD_ID} ."
                sh "docker image push shaikkhajaibrahim/orderapp:dev_${BUILD_ID}"

            }
        }
    }
}