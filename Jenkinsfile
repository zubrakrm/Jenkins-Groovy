node{
      def dockerImageName= 'intdoc89/javadedockerapp_$JOB_NAME:$BUILD_NUMBER'
      stage('SCM Checkout'){
         git 'https://github.com/zafar90/java-groovy-docker'
      }
      stage('Build'){
         // Get maven home path and build
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'   
         sh "/opt/apache-maven-3.6.3/bin/mvn package -Dmaven.test.skip=true"
      }       
     
     stage ('Test'){
         //def mvnHome =  tool name: 'Maven 3.5.4', type: 'maven'    
         sh "/opt/apache-maven-3.6.3/bin/mvn verify; sleep 3"
      }
      
     stage('Build Docker Image'){         
           sh "docker build -t ${dockerImageName} ."
      }  
   
      stage('Publish Docker Image'){
         withCredentials([string(credentialsId: 'dockerpwd', variable: 'dockerPWD')]) {
              sh "docker login -u intdoc89 -p ${dockerPWD}"
         }
        sh "docker push ${dockerImageName}"
      }
      
    stage('Run Docker Image'){
            def dockerContainerName = 'javadockerapp_$JOB_NAME_$BUILD_NUMBER'
            def changingPermission='sudo chmod +x stopscript.sh'
            def scriptRunner='sudo ./stopscript.sh'           
            def dockerRun= "sudo docker run -p 8082:8080 -d --name ${dockerContainerName} ${dockerImageName}" 
            withCredentials([string(credentialsId: 'deploymentserverpwd', variable: 'dpPWD')]) {
              sh "${dockerRun}"
            }
            
      
      }
      
         
  }
