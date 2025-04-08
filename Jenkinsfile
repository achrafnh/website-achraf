pipeline {
	agent any

stages{

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
            withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD_ACHRAF', variable: 'DOCKER_HUB_PASSWORD')]) {
                
                sh 'docker login -u hrefnhaila -p $DOCKER_HUB_PASSWORD'
                sh 'docker build -t hrefnhaila/devops-mywebsite:v1 .'
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
}





