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
        stage ('Deploy') {
            steps {
               //sh label: '', script: 'curl -u admin:admin --upload-file dist.zip http://18.191.215.188:8080/manager/text/deploy?config=file:/var/lib/tomcat8/webapps/'
              // sh 'ssh http://18.191.215.188:8080/manager  ls'
               withCredentials([file(credentialsId: 'atlassian-tools', variable: 'password')]) {
                  // sh 'ssh -i ${atlassian-tools} ubuntu@18.191.215.188 unzip "angularAnkush/dist.zip" && ls'
                  sh 'scp -i ${secret_key_for_tomcat} build.zip ubuntu@18.191.215.188:/var/lib/tomcat8/webapps/'
                  sh 'ssh -i ${secret_key_for_tomcat} ubuntu@18.191.215.188 "cd /var/lib/tomcat8/webapps/;mkdir arko489;unzip -o build.zip -d ./build;"'
               }
            }
        }
       
       
    }
}
