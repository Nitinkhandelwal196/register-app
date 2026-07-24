pipeline {
     agent { label 'Jenkins-Agent' }
     tools {
          jdk 'Java21'
          maven 'maven3'
           }
  stages { 
      stage ("Cleanup Workspace"){
              steps{
                cleanWs()
                   }
              }
      stage ("Checkout from SCM"){
               steps{
                 git branch: 'main' ; credentialsId: 'github', url: 'https://github.com/Nitinkhandelwal196/register-app.git'
                   }
               }
      stage ("Build Application"){
                 steps{
                        sh "mvn clean package"
                   }
               }
      stage ("Test Application"){
                 steps{
                     sh "mvn test"
                   }
               }
  }
}
