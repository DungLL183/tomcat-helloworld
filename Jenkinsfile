
node{
  stage("Scm checkout"){
    git 'https://github.com/DungLL183/tomcat-helloworld.git'
  }

  stage("Maven build"){
    sh 'mvn clean package'
  }

  stage("Build docker image"){
    sh 'docker build -t dungll183/docker-tomcat:v4 .'
  }

  stage("Push docker image"){
    withCredentials([string(credentialsId: 'creds-docker', variable: 'dockerHubPwd')]) {
          sh "docker login -u dungll183 -p ${dockerHubPwd}"
	sh 'docker push dungll183/docker-tomcat:v4'
  }
  stage("Remove"){
    def dockerRm = 'docker container rm -f my-app-2'
    sshagent(['dev-server']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.3 ${dockerRm}"
	}
}
  stage("Deploy docker image to Tomcat server"){
    def dockerRun = 'docker run -p 8080:8080 -d --name my-app-2 dungll183/docker-tomcat:v4'
    sshagent(['dev-server']) {
      sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.12.3 ${dockerRun}"
    }
  }
}
}
