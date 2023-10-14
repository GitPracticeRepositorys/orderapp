pipeline {
    agent any 

    stages {
        stage('checkout') {
            steps {
                git url: 'https://github.com/GitPracticeRepo/orderapp.git',
                    branch: main
            }
            
        }
        stage('build and push the image ') {
            steps {
                script {
                    def imageTag = "shaikkhajaibrahim/dev_${env.BUILD_NUMBER}"
                    sh "docker image build -t ${imageTag} ."
                    sh "docker image push ${imageTag}"

                } 


            }
        }
    }
}