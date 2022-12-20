node{
     
    stage('SCM Checkout'){
        git url: 'https://github.com/MithunTechnologiesDevOps/java-web-app-docker.git',branch: 'master'
    }
    
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven-3.5.6", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
      
    } 
    
    
    stage('Build Docker Image'){
        sh 'docker build -t anudeepb/java-web-app-docker .'
    }
    
    stage('Push Docker Image'){
        withCredentials([string(credentialsId: 'Docker_Hub_Passowrd', variable: 'Docker_Hub_Paswword')]) {
          sh "docker login -u anudeepb -p ${Docker_Hub_Password}"
        }
        sh 'docker push anudeepb/java-web-app-docker'
     }
     
      stage('Run Docker Image In Dev Server') {
           
        def dockerRun = ' docker run -d -p 8080:8080 --name java-web-app anudeepb/java-web-app-docker:1'
          
          
        
           sshagent(['Docker_Server']) {  
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.208 docker run -d -p 8080:8080 --name javawebappcontainer anudeepb/java-web-app-docker:1"
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.47.208 docker rm -f javawebappcontainer || true"
       
       
    }
     
     
}
