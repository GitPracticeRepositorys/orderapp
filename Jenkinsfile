pipeline {
    agent {docker-node-1}

    stages {
        stage('checkout') {
            steps {
                git url: 'https://github.com/GitPracticeRepositorys/orderapp.git',
                    branch: "main"
            }
            
        }
        stage('build and push the image ') {
            steps {
                sh "docker image build -t shivakrishna99/orderapp:dev_${BUILD_ID} ."
                sh "docker image push shivakrishna99/orderapp:dev_${BUILD_ID}"

            }
        }
        stage('update k8s manifest') {
            steps {
                sh "cd ~/orderopsk8s && yq eval -i '.spec.template.spec.containers[0].image = \"shivakrishna99/orderapp:dev_${BUILD_ID}\" ' ~/orderopsk8s/manifests/orderdeploy.yaml"
                sh """
                  cd ~/orderopsk8s
                  git add ~/orderopsk8s/manifests/orderdeploy.yaml
                  git commit -m "added new change"
                  git push origin main
                """
                
            }
        }
    }
}
