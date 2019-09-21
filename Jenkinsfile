pipeline {
  environment {
      registry = "alex1311/udacitycapstone"
  }
     agent any
     stages {
     //stage('Lint HTML check') {
         //steps {
	     //sh 'echo "Check lint for HTML"'
         //sh 'tidy -q -e *.html'
         //}
      //}
	 stage ('Build the image.'){
         steps {
             script {
              withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']])
              {
              sh '''sudo docker build -t alex1311/udacitycapstone:$BUILD_ID .'''
              }
             }
         }
     }
     stage ('Push image') {
	     steps {
		 script {
           docker.withRegistry('',registryCredential ) {
               sh 'sudo docker push alex1311/udacitycapstone:latest'
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
