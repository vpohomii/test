pipeline {
  environment { 
    registry = "https://pohomiy.jfrog.io/artifactory/"
    
    registryCredential = 'jfusernamepass'
    registryCredential1 = 'jftoken'
    registryCredential2 = 'jf'
    registryCredentiald = 'dh1'
    imagename = 'vnp79/pyapp'
    dockerImage = ''
    relase = "0.${env.BUILD_NUMBER}"
  }   
  agent {
    kubernetes { 
    label 'docker'
    yamlFile './yaml/docker.yaml'
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
                dockerImage.push("${env.BUILD_NUMBER}")
                }
              }
            }
        }

        stage('Prepare to deploy To K8s') {
            steps{
              sh "sed -i 's/latest/${env.BUILD_NUMBER}/g' ./yaml/deploy.yaml"
              sh 'cp ./yaml/deploy.yaml deploy.yaml'      
            }
        }

        stage('K8s Rolling Update'){
            steps{
              script {
                 // withCredentials([file(credentialsId: 'k8s', variable: 'KUBECONFIG')]) {
                  // change context with related namespace
                //  sh '''kubectl config set-context $(kubectl config current-context)'''
                kubernetesDeploy(configs: "deploy.yaml", kubeconfigId: "k8s")
                
              }
            }
        }
    }            
}
