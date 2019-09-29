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
                  //sh 'docker run -d -p 80:80 alex1311/udacitycapstone:$BUILD_ID'
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

     stage('Deploy image to EKS cluster') {
      steps {
        withAWS(region:'us-west-2', credentials:'capstone-user') {
	  //sh '''aws iam get-user'''
          //echo 'Deploying....'
          //withKubeConfig(credentialsId: 'eks-kubeconfig', serverUrl: 'https://2854F248FA6F069EC0B7B8C96E0AAFCB.gr7.us-west-2.eks.amazonaws.com') {
            sh '''aws --region us-west-2 eks update-kubeconfig --name eksworkshop-cf'''
	    sh '''kubectl get nodes'''
 	    sh '''kubectl run capstonedeployment --image=alex1311/udacitycapstone:"$BUILD_ID"'''
            //sh '''kubectl set image deployments/capstonedeployment capstonedeployment=alex1311/udacitycapstone:"$BUILD_ID"'''
	    sh '''kubectl rollout status -w deployment/capstonedeployment'''
            sh '''kubectl get pods'''
          //}
        }
      }



     }
     stage('Remove Unused docker image') {
      steps{
       sh "docker rmi $registry:$BUILD_NUMBER"
      }
     }
}
}
