pipeline {
    agent { label 'docker-node-1' }

    stages {
        stage('Checkout OrderApp') {
            steps {
                script {
                    checkout([$class: 'GitSCM', branches: [[name: 'dev']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: 'https://github.com/GitPracticeRepositorys/orderapp.git']]])
                }
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_NUMBER}"
                    sh "docker image build -t $imageName ."
                    sh "docker image push $imageName"
                }
            }
        }

        stage('Update Kustomize and Push') {
            steps {
                agent { label 'docker-node' }
                script {
                    def kustomizeRepoDir = env.HOME + "/orderopsk8s"

                    // Update the image in Kustomize
                    sh """
                        /usr/local/bin/kustomize edit set image my-container=shivakrishna99/orderapp:dev_${BUILD_NUMBER}
                        cd ${kustomizeRepoDir}
                        git config --global user.email "knowledgesk9999@gmail.com"
                        git config --global user.name "GitPracticeRepositorys"
                        git add .
                        git commit -m "Update container image"
                        git push origin dev
                    """
                }
            }
        }
    }
}
