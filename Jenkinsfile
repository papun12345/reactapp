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
       
       
       
    }
}
