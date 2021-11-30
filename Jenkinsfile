node {
    def app

    stage('Clone repository') {
        /* Let's make sure we have the repository cloned to our workspace */

        checkout scm
    }

     stage('sleepy') {
    
    sh  'apt-get update && apt install apt-transport-https ca-certificates software-properties-common curl && curl -fsSL https://download.docker.com/linux/ubuntu/gpg | apt-key add - && sudo add-apt-repository  "deb [arch=amd64] https://download.docker.com/linux/ubuntu  $(lsb_release -cs) stable" && apt-get update && apt-get install docker-ce'
    }
    
    stage('Build image') {
        /* This builds the actual image; synonymous to
         * docker build on the command line */

        app = docker.build("vnp79/demo")
    }
}
