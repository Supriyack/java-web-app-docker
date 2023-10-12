node{
     def buildNumber = BUILD_NUMBER
    stage('SCM Checkout'){
        git url: 'https://github.com/Supriyack/java-web-app-docker.git',branch: 'master'
    }
    stage(" Maven Clean Package"){
      def mavenHome =  tool name: "Maven", type: "maven"
      def mavenCMD = "${mavenHome}/bin/mvn"
      sh "${mavenCMD} clean package"
       } 
    stage("Build Dokcer Image") {
         sh "docker build -t supriyack/java-web-app:${buildNumber} ."
    }
    stage("Docker login and Push"){
        withCredentials([string(credentialsId: 'Docker_Hub_Pwd', variable: 'Dockerpassword')])
        {
         sh "docker login -u supriyack -p ${Dockerpassword} " 
        }
        
        sh "docker push supriyack/java-web-app:${buildNumber}"
    }
   stage('Deploy to dockercontainer in docker deployer'){
       sshagent(['docker_ssh_password']) {
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.16.137 docker rm -f cloudcandy || true"
        sh "ssh -o StrictHostKeyChecking=no ubuntu@172.31.16.137 docker run -d -p 8080:8080 --name cloudcandy supriyack/java-web-app:${buildNumber}"           
   
    
}
   }

}
