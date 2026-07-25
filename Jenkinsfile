pipeline {
     agent { label 'Jenkins-Agent' }
     tools {
          jdk 'Java17'
          maven 'maven3'
           }
       environment {
	    APP_NAME = "register-app-pipeline"
            RELEASE = "1.0.0"
            DOCKER_USER = "dockerinfo196"
            DOCKER_PASS = 'DockerHub'
            IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
            IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
	    
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
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }

                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }

       }
	   stage("Trivy Scan") {
           steps {
               script {
	            sh ('docker run -v /var/run/docker.sock:/var/run/docker.sock aquasec/trivy image dockerinfo196/register-app-pipeline:latest --no-progress --scanners vuln  --exit-code 0 --severity HIGH,CRITICAL --format table')
               }
           }
       }

	       stage ('Cleanup Artifacts') {
           steps {
               script {
                    sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG}"
                    sh "docker rmi ${IMAGE_NAME}:latest"
               }
          }
       }



       
                         
  }
}
