pipeline {
    agent any
    //environment {
        // comment added
        // APPLICATION = 'app'
        // ENVIRONMENT = 'dev'
        // MAINTAINER_NAME = 'jenkins'
        // MAINTAINER_EMAIL = 'jenkins@email.com'
    //}
      stages {
         stage('clone repository') {
             steps {
                 // cloning repo
                 git credentialsId: 'sree-github', url: 'https://github.com/srinivasareddy4218/Movies-App1.git'
             }
         }
         stage('Build Docker Image'){
            steps{
                sh "sudo docker build . -t srinivasareddy4218/movies-app:${BUILD_ID} "
            }
        }
        stage('DockerHub Push'){
            steps{
               withCredentials([string(credentialsId: 'sree-docker', variable: 'sample')]) {
                    sh "sudo docker login -u srinivasareddy4218 -p ${sample}"
                    sh "sudo docker push srinivasareddy4218/movies-app:${BUILD_ID}"
                }
            }
        }
         stage('Build Image') {
             steps {
                  sshagent(credentials : ['sshkey']) {
                      sh " scp -o StrictHostKeyChecking=no  /var/lib/jenkins/workspace/movies-app/frontend/deployment/frontend-deployment.yaml azureuser@40.117.94.170:/home/azureuser"
                         script{
                             try{
                              sh "ssh azureuser@40.117.94.170 kubectl apply -f ."
                             }catch (error){
                               sh "ssh azureuser@40.117.94.170 kubectl create -f ."
                         //sh "whoami && pwd"
                         //sh 'ssh -t -t azureuser@40.117.94.170 -o StrictHostKeyChecking=no'
                         //sh "ls -la"
            
                     }                                      
                        
            }
          }
       }
     }
   }
 }
    
  
