pipeline {
  environment {
    registry = "pohomiy.jfrog.io/artifactory/"
    registryCredential = 'jfusernamepass'
    registryCredential1 = 'jftoken'
    registryCredential2 = 'jf'
    imagename = 'pohomiy.jfrog.io/artifactory/fine-docker-local/fine-docker'
    dockerImage = ''
  }   
  agent {
    kubernetes { 
        yaml '''  
    apiVersion: v1
    kind: Pod
    spec:
     containers:    
     - name: docker
       image: docker:19.03.1-dind
       securityContext:
         privileged: true  
        '''
      defaultContainer 'docker'
    }
  }
    stages {
       stage('conf') {
           steps {
             rtServer (
             id: "jFrog",
             url: "https://pohomiy.jfrog.io/artifactory/",
             credentialsId: "jftoken"
           )    
           }
       }
      
       stage('git') {
             steps {
               git branch: 'main', url: 'https://github.com/vpohomii/test.git'
             }
       }

        stage('docker'){
            steps {
              script {
                 dockerImage = docker.build imagename 
              }
            }
        }
        stage('Deploy Image') {
            steps{
              script {
                docker.withRegistry( registry, registryCredential ) {
                dockerImage.push("0.$BUILD_NUMBER")
                dockerImage.push('latest')
                }
              }
            }
        }  
      }            
}
