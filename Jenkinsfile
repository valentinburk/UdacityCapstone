pipeline {
  environment {
      registry = "alex1311/udacitycapstone"
  }
     agent any
     stages {
     stage('Lint HTML check') {
         steps {
	     sh 'echo "Check lint for HTML"'
         sh 'tidy -q -e *.html'
         }
      }
	 stage ('Build the image.'){
         steps {
             script {
                 dockerImage = docker.build registry + ":$BUILD_NUMBER"
             }
         }
     }
     stage ('Push image') {
	     steps {
		 script {
           docker.withRegistry('',registryCredential ) {
               dockerImage.push("latest")
           }
          }
	     }
	   }
     //stage('Remove Unused docker image') {
      //steps{
       // sh "docker rmi $registry:$BUILD_NUMBER"
      //}
     //}
}
}
