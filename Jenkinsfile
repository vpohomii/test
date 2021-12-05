pipeline {
  environment {
    registry = "https://pohomiy.jfrog.io/artifactory/"
    
    registryCredential = 'jfusernamepass'
    registryCredential1 = 'jftoken'
    registryCredential2 = 'jf'
    registryCredentiald = 'dh'
    imagename = 'vnp79/pyapp'
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
               git branch: 'kOps', url: 'https://github.com/vpohomii/test.git'
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
                docker.withRegistry( '', registryCredentiald ) {
                dockerImage.push("0.${env.BUILD_NUMBER}")
                dockerImage.push('latest')
                }
              }
            }
        }  
      }            
}
