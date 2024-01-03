pipeline {
    agent { label 'docker-node-1' }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_NUMBER}"
                    def orderopsk8sDir = env.HOME + "/orderopsk8s"

                    sh "docker image build -t $imageName ."
                    sh "docker image push $imageName"

                    // Path to the kustomization.yaml file
                    def kustomizeDir = "${orderopsk8sDir}/kustomize"

                    // Use Kustomize to update the image in the Kubernetes manifests
                    sh """
                        /usr/local/bin/kustomize edit set image my-container=${imageName}
                        cd ${kustomizeDir}
                        git add .
                        git diff --quiet || git commit -m 'Update container image'
                        git push origin dev
                    """
                }
            }
        }
    }
}
