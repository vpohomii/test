pipeline {
  environment {
    registry = "pohomiy.jfrog.io/artifactory/"
    registryCredential = 'jfusernamepass'
    registryCredential1 = 'jftoken'
    image = 'pohomiy.jfrog.io/artifactory/fine-docker-local/fine-docker'
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
                 MyImage = docker.build('pohomiy.jfrog.io/artifactory/fine-docker-local/fine-docker:0.${env.BUILD_NUMBER}')
              }
            }
        }
        stage ('Push image to Artifactory') {
            steps {
              rtDockerPush(
                    serverId: "jFrog",
                    image: 'pohomiy.jfrog.io/artifactory/fine-docker-local/fine-docker:0.${env.BUILD_NUMBER}',
                    targetRepo: 'fine-docker-local',
                )
                }
            }
        stage ('Publish build info') {
            steps {
                rtPublishBuildInfo (
                    serverId: "jFrog"
                )
            }
        }



        }            

    }    
}
