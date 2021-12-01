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
        stage('git') {
            steps {
                git branch: 'main', url: 'https://github.com/vpohomii/test.git'
            }
        }
        stage('docker'){
            steps {
                 sh 'docker version && pwd && ls -la  && docker build -t demo:1.${BUILD_NUMBER} .'
                  sh 'echo  Hello Docker! '
            }
        }
    }
}
