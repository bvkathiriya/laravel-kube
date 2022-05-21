pipeline {

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/bvkathiriya/laravel-kube.git'
      }
    }
     stage('Build Docker Image'){
            steps{
                 sh 'docker version'
                 sh 'docker build -t sanjay-docker-new .'
                 sh 'docker image list'
                 sh 'docker tag sanjay-docker-new kathiriya007/laravel-kube:${BUILD_NUMBER}'
            }
        } 
        stage('docker image Push'){
            steps{
                withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
                 sh 'docker login -u kathiriya007  -p $PASSWORD'
                 sh 'docker push kathiriya007/laravel-kube:${BUILD_NUMBER}'
                }
            }    
        

        } 

    

   stage('Deploy to K8s') {
      steps {
        withKubeConfig([credentialsId: 'kubernetes']) { 
          sh 'sed -i "s/<TAG>/${BUILD_NUMBER}/" deployment.yaml'
          sh 'kubectl apply -f deployment.yaml  --validate=false'
          sh 'kubectl apply -f service.yaml'
        }
      } 
    }


  }
}
