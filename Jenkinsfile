pipeline {
  agent none
	triggers {
    pollSCM 'H/5 * * * *'
  }
  stages {
    stage('Build Image & Upload') {
      when {
        branch "master"
      }
      agent {
        label 'docker'
      }
      steps {
          /*withCredentials([usernamePassword(credentialsId: 'docker-login', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USER')]) {
              sh './mvnw package -Dmaven.test.skip=true'
              sh 'docker build -t baoziface/auth-service:v1 ./auth-service'
              sh 'docker login --username=$DOCKER_USER --password=$DOCKER_PASSWORD'
              sh 'docker push  baoziface/auth-service:v1'
              }*/
          }
    }
    stage('deploy') {
	      when {
	     	branch "master"
	      }
	      agent {
		    label 'docker'
	      }
	      steps {
            withKubeConfig([
              credentialsId: 'k8s',
              contextName: 'minikube'
            ]) {
              sh 'kubectl apply -f ./account-service/account-service.yaml'
            }
	      }
	    }
	stage('deploy for qa'){
	     steps{
		    input message: 'deploy for qa?'
	    }
	}
  }
}
