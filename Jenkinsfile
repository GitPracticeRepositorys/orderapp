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
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_ID}"
                    def orderopsk8sDir = "/home/ubuntu/orderopsk8s"
                    
                    sh "docker image build -t $imageName ."
                    sh "docker image push $imageName"
                    
                    sh """
                        yq eval -i '.spec.template.spec.containers[0].image = \"${imageName}\"' ${orderopsk8sDir}/manifests/orderdeploy.yaml
                        cd ${orderopsk8sDir}
                        git add manifests/orderdeploy.yaml
                        git commit -m "Added new change"
                        git push origin main
                    """
                }
            }
        }
    }
}
