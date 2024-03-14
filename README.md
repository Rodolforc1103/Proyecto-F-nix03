# Proyecto-F-nix03
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                retry(3) {
                    checkout scm
                }
            }
        }
        stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            emailext(
                subject: "Build Success: ${currentBuild.fullDisplayName}",
                body: "The build ${currentBuild.fullDisplayName} succeeded.",
                to: "rodoracardales@gmail.com"
            )
        }
        failure {
            emailext(
                subject: "Build Failure: ${currentBuild.fullDisplayName}",
                body: "The build ${currentBuild.fullDisplayName} failed.",
                to: "rodoracardales@gmail.com"
            )
        }
    }
}


      
   
        


