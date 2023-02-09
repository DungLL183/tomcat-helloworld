node{
    agent any
	
	tools
    {
       maven 'maven'
    }
    stage('checkout') {
                git branch: 'https://github.com/DungLL183/docker-tomcat.git'
        }
	stage('Execute Maven') {
                sh 'mvn clean package'             
      
        }
  	stage('Docker Build') {
          
                sh 'docker build -t samplewebapp:v4 .'
          
        }
	 
	 stage('docker push'){
		 withCredentials([string(credentialsId: 'haha', variable: 'dockerPwd')]) {
			 sh 'docker login -u dungll183 -p ${dockerPwd}'
		}
		sh 'docker push dungll183/samplewebapp:v4
	 }
     
 	stage('Run Docker container on remote hosts') {
		def dockerRun = 'docker run -it --name my-application -p 8080:8080 -d dungll183/samplewebapp:v4'
		sshagent(['ssh-ec2']) {
                sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.3 ${dockerRun}
		}
	}
    }
}
    
