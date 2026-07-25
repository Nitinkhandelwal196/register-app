pipeline {
     agent { label 'Jenkins-Agent' }
     tools {
          jdk 'Java17'
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
                 git branch: 'main' , credentialsId: 'github', url: 'https://github.com/Nitinkhandelwal196/register-app.git'
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
       stage ("SonarQube Analysis"){
                 steps{
                      script{
                           withSonarQubeEnv( 'SonarQube-Server'){
                                sh "mvn org.sonarsource.scanner.maven:sonar-maven-plugin:sonar"
                           }
                      }
                 }
       }
       stage("Quality Gate") {
    steps {
        timeout(time: 5, unit: 'MINUTES') {
            waitForQualityGate abortPipeline: true
        }
    }
}
       
                         
  }
}
