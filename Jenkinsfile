pipeline {
	agent any

  stage('Generate Local Tag') {
    steps {
        script {
            def shortHash = sh(script: 'git rev-parse --short HEAD', returnStdout: true).trim()
            def timestamp = new Date().format('yyyyMMddHHmmss')
            env.LOCAL_TAG = "v${timestamp}-${shortHash}"

            echo "Generated Local Tag: ${env.LOCAL_TAG}"
        }
    }
}

    stage('Docker Build & Push') {
        steps {
            withCredentials([string(credentialsId: 'docker-hub-password', variable: 'DOCKER_HUB_PASSWORD')]) {
                script {
                    IMAGE_NAME = "hrefnhaila/devops-app:${env.LOCAL_TAG}"
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
                    sh "sed -i 's#replace#hrefnhaila/devops-app:${env.LOCAL_TAG}#g' k8s_deployment_service.yaml"
                    sh 'kubectl apply -f k8s_deployment_service.yaml'
                }
            }
        }
    }

}





