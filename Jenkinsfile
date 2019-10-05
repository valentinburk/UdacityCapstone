pipeline {
  environment {
      registry = "alex1311/udacitycapstone"
      registryCredential = 'dockerhub'
  }
     agent any
     stages {

   stage ('Cloning git repo') {
    steps{
      git 'https://github.com/alexba13/UdacityCapstone.git'
    }
   }

  stage ('Linting Dockerfile before build') {
    steps{
      sh '''docker run --rm -i hadolint/hadolint < Dockerfile'''
    }
   }

	 stage ('Build the image.'){
         steps {
             script {
              withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'dockerhub', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD']])
              {
                  dockerImage=docker.build registry + ":$BUILD_ID"
                  sh 'docker run -d --name capstone_container -p 80:80 alex1311/udacitycapstone:$BUILD_ID'
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
            sh '''aws --region us-west-2 eks update-kubeconfig --name UdacityEKSCapstone'''
	          sh '''kubectl get nodes'''
            //For the first deployment, uncomment the below command
 	          //sh '''kubectl run capstonedeployment --image=alex1311/udacitycapstone:"$BUILD_ID" --port=80 --expose=true'''
            sh '''kubectl set image deployments/capstonedeployment capstonedeployment=alex1311/udacitycapstone:"$BUILD_ID"'''
	          sh '''kubectl rollout status -w deployment/capstonedeployment'''
	          sh '''kubectl scale deployments/capstonedeployment --replicas=3'''
	          sh '''kubectl get pods -o wide'''
	          sh '''kubectl describe deployment capstonedeployment'''
        }
      }
     }
     stage('Remove Unused docker image') {
      steps{
       sh '''docker ps -a'''
       sh '''docker stop capstone_container'''
       sh '''docker rm $(docker ps -a -q)'''
       sh '''docker rmi $registry:$BUILD_ID'''
       sh '''docker ps -a'''
      }
     }
  }
}
