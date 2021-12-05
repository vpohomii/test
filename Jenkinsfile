pipeline {
  environment {
    registry = "pohomiy.jfrog.io/artifactory/"
    registryCredential = 'jftoken'
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
                 docker.build registry + ":1.+$BUILD_NUMBER"
              }
            }
        }
        stage ('Push image to Artifactory') {
            steps {
                script {
                  docker.withRegistry("pohomiy.jfrog.io/artifactory/", "jftoken") {
                    docker.push("1.${env.BUILD_NUMBER}")
                    docker.push("latest")  

                  }
                }
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
