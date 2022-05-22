pipeline {
    environment { 

        registry = "kathiriya007/laravel-kube" 

        registryCredential = 'DOCKER_HUB_PASSWORD' 

        dockerImage = '' 

    }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git branch: 'main', url: 'https://github.com/bvkathiriya/laravel-kube.git'
      }
    }
     stage('Building our image') { 

            steps { 

                script { 

                    dockerImage = docker.build registry + ":$BUILD_NUMBER" 

                }

            } 

        }

        stage('Deploy our image') { 

            steps { 

                script { 

                    docker.withRegistry( '', registryCredential ) { 

                        dockerImage.push() 

                    }

                } 

            }

        } 

        stage('Cleaning up') { 

            steps { 

                sh "docker rmi $registry:$BUILD_NUMBER" 

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
