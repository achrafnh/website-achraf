pipeline {
	agent any





    stage('Docker Build & Push') {
        steps {
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD_ACHRAF', variable: 'DOCKER_HUB_PASSWORD')]) {
                script {
                    IMAGE_NAME = "hrefnhaila/devops-mywebsite:v1"
                }
                sh 'docker login -u hrefnhaila -p $DOCKER_HUB_PASSWORD'
                sh 'docker build -t $IMAGE_NAME .'
                sh 'docker push $IMAGE_NAME'
            }
        }
    }

 

    stage('Kubernetes Deployment') {
        steps {
            withKubeConfig([credentialsId: 'kubeconfigachraf']) {
                script {
                    sh "sed -i 's#replace#hrefnhaila/devops-mywebsite:v1#g' k8s_deployment_service.yaml"
                    sh 'kubectl apply -f k8s_deployment_service.yaml'
                }
            }
        }
    }

}





