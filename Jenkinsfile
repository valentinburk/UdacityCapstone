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
   stage ('Cloning git repo') {
    steps{
      git 'https://github.com/alexba13/UdacityCapstone.git'
    }
   }
	 stage ('Build the image.'){
         steps {
             script {
              withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']])
              {
                  //sh '''sudo docker build -t alex1311/udacitycapstone:$BUILD_ID .'''
                  dockerImage=docker.build registry + ":$BUILD_NUMBER"
                  sh 'docker run --name nginx -d -P alex1311/udacitycapstone:$BUILD_ID'
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
