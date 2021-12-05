pipeline {
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
             credentialsId: "jf-token"
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
                 docker.build('pohomiy.jfrog.io/artifactory/fine-docker-local/fine-docker')
              }
            }
        }
        stage ('Push image to Artifactory') {
            steps {
                script {
                  docker.withRegistry('pohomiy.jfrog.io/artifactory/', 'jf-token') {
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
