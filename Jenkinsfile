pipeline {
    agent any
    
    tools {nodejs "nodejs"}
   
    
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
                
            }
        }
    

       stage('Test') {
            steps {
                sh 'npm run test'
                sh 'npm test -- --coverage'
            }
        }
         stage ('zipping'){
            steps {
                sh 'npm run build'
                sh 'zip -r build.zip ./build'
            }
        }
       
      
        stage('Sonarqube analysis') {
               environment {
                 scannerHome=tool 'sonar scanner'
            }
            
              steps {
              sh 'npm test -- --coverage'
              sh '${scannerHome}/bin/sonar-scanner -Dproject.settings=./sonar.properties'
                  }
        }
       stage("Quality Gate"){
       timeout(time: 1, unit: 'HOURS') { // Just in case something goes wrong, pipeline will be killed after a timeout
      def qg = waitForQualityGate() // Reuse taskId previously collected by withSonarQubeEnv
    if (qg.status != 'OK') {
        error "Pipeline aborted due to quality gate failure: ${qg.status}"
    }
  }
}
          
    

        stage ('Uploading artifact to nexus'){
        steps{
        withCredentials([usernamePassword(credentialsId: 'arko489_nexus', passwordVariable: 'pwd', usernameVariable: 'usr')]) {
         sh 'curl -v -u $usr:$pwd --upload-file build.zip http://3.14.251.87:8081/nexus/content/repositories/devopstraining/arko/build.zip'
            }
        }
       }
    }
        post {
    success {
      slackSend (color: '#00FF00', message: "SUCCESSFUL: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
    failure {
      slackSend (color: '#FF0000', message: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]' (${env.BUILD_URL})")
    }
  }
}
