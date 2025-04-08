pipeline {
	agent any

	stages {
	    //---------------------------------------
    	stage('git version') {
        		steps {
        		    sh 'git version'
        		    
        		}
    	}
    	  //---------------------------------------
    	stage('docker version') {
        		steps {
        		    sh 'docker -v'	
        		    
        		}
    	}
    	  //---------------------------------------
   	 
   	stage('kubernetes version') {
        		steps {   
        		    withKubeConfig([credentialsId: 'kubeconfigachraf']) {
        		    sh 'kubectl version --client'  	
        		    }
        		    
        		}
    	}
  //---------------------------------------
	}
}


