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
        stage('update k8s manifest') {
            steps {
                sh "cd ~/orderopsk8s && yq eval -i '.spec.template.spec.containers[0].image = \"shaikkhajaibrahim/orderapp:dev_${BUILD_ID}\" ' ~/orderopsk8s/manifests/orderdeploy.yaml"
                sh """
                  cd ~/orderopsk8s
                  git add ~/orderopsk8s/manifests/orderdeploy.yaml
                  git commit -m "added new change"
                  git push origin master
                """
                
            }
        }
    }
}