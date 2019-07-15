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
            }
        }
         stage ('zipping'){
            steps {
                sh 'npm run build'
                sh 'zip -r build.zip ./build'
            }
        }
        stage ('Uploading artifact to nexus'){
         steps{
         withCredentials([usernamePassword(credentialsId: 'arko489_nexus', passwordVariable: 'pwd', usernameVariable: 'usr')]) {
         sh label: '', script: 'curl -v -u $usr:$pwd --upload-file build.zip http://3.14.251.87:8081/nexus/content/repositories/devopstraining/arko/build.zip'
            }
        }
        }
      
        stage('Sonarqube analysis') {
        steps {
            sh 'npm install sonarqube-scanner --save-dev'
            sh 'npm run sonar'
           }
        }
       
       
    }
}
