node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/gokulbair/java-web-app-docker.git',branch: 'master'
    }
     stage(" Maven Clean Package"){
      def mavenHome =  tool name: "maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
     stage('Build Docker Image'){
        sh 'docker build -t gokumaxi/java-web-app .'
    }
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerpwd')]) {
          sh "docker login -u gokumaxi -p ${dockerpwd}"
        }
        sh 'docker push gokumaxi/java-web-app'
     }
     
     stage('Run Docker Image In Dev Server'){
        
        def dockerRun = 'docker run  -d -p 8080:8080 --name java-web-app gokumaxi/java-web-app'
         
         sshagent(['DOCKER_SERVER']) {
          sh 'ssh -o StrictHostKeyChecking=no ubuntu@172.31.45.41 docker stop java-web-app || true'
          sh 'ssh  ubuntu@172.31.45.41 docker rm java-web-app || true'
          sh 'ssh  ubuntu@172.31.45.41 docker rmi -f  $(docker images -q) || true'
          sh "ssh  ubuntu@172.31.45.41 ${dockerRun}"
       }
       
    }
     
    
}
