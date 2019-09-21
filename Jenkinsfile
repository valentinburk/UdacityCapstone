pipeline {
  environment {
      registry = "alex1311/udacitycapstone"
      registryCredential = 'dockerhub'
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
                  //sh '''sudo docker build -t alex1311/udacitycapstone:$BUILD_ID .'''
                  docker.build registry + ":$BUILD_NUMBER"
              }
             }
         }
     }
     stage ('Push image') {
	     steps {
		 script {
           docker.withRegistry('',registryCredential ) {
               dockerImage.push()
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
