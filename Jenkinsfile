currentBuild.displayName="movies-app-#"+currentBuild.number
pipeline {
    agent any
    environment{
        DOCKER_TAG = getDockerTag()
    }
    stages{
        stage('Build Docker Image'){
            steps{
                sh "sudo docker build . -t srinivasareddy4218/movies-app:${DOCKER_TAG} "
            }
        }
        stage('DockerHub Push'){
            steps{
               withCredentials([string(credentialsId: 'sree-Docker-pwd', variable: 'sample')]) {
                    sh "sudo docker login -u srinivasareddy4218 -p ${sample}"
                    sh "sudo docker push srinivasareddy4218/movies-app:${DOCKER_TAG}"
                }
            }
        }
        stage("Deploy To Kuberates Cluster"){
	    sh "sed -i -e 's,image_to_be_deployed,'srinivasareddy4218/movies-app:${BUILD_ID}',g' frontend-deployment.yaml"
	      
        sh "export KUBECONFIG=/etc/kubernetes/admin.conf && kubectl apply -f frontend-deployment.yaml -n msslabs"
	
      }

   }
} 
def getDockerTag(){
    def tag  = sh script: 'git rev-parse HEAD', returnStdout: true
    return tag
    }
 
