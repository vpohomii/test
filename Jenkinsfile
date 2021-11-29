pipeline { 
     agent any
    
    triggers {
         pollSCM ('H/5 * * * *')  
    }

    stages {
        stage("checkout") {
            steps { 
                checkout scm
            }
        }
        stage("Building Docker Image") {
                    steps {
                        dockerfile {
                        filename 'Dockerfile'
                        label 'demo-vn'
                        }
                    }    
        }        
        stage("testing") {
            steps { 
                prinln: "Hello Docker!"
            }
        } 
    }
}
