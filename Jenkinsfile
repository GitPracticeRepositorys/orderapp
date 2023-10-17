pipeline {
    agent { label 'docker-node-1' }

    stages {
        stage('checkout') {
            steps {
                git url: 'https://github.com/GitPracticeRepositorys/orderapp.git',
                    branch: "main"
            }
        }

        stage('build and push the image') {
            steps {
                script {
                    def imageName = "shivakrishna99/orderapp:dev_${BUILD_ID}"
                    sh "docker image build -t $imageName ."
                    sh "docker image push $imageName"
                }
            }
        }

        stage('update k8s manifest') {
            steps {
                script {
                    def orderopsk8sDir = "/home/ubuntu/orderopsk8s"
                    sh """
                      yq eval -i '.spec.template.spec.containers[0].image = \"shivakrishna99/orderapp:dev_${BUILD_ID}\" ' $orderopsk8sDir/manifests/orderdeploy.yaml
                    """
                    sh """
                      cd $orderopsk8sDir
                      git add manifests/orderdeploy.yaml
                      git commit -m "added new change"
                      git push origin main
                    """
                }
            }
        }
    }
}
