# Proyecto-F-nix03
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                // Checkout code from Git
                git 'https://github.com/Rodolforc1103/Proyecto-F-nix03.git'
            }
        }
        stage('Build') {
            steps {
                // Build the Java application
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                // Run tests
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                // Deploy the application (replace with your deployment script)
                sh './deploy.sh'
            }
        }
    }

    post {
        success {
            // Send notification if the build succeeds
            emailext(
                subject: "Build Success: ${currentBuild.fullDisplayName}",
                body: "The build ${currentBuild.fullDisplayName} succeeded.",
                to: "rodoracardales@gmail.com"
            )
        }
        failure {
            // Send notification if the build fails
            emailext(
                subject: "Build Failure: ${currentBuild.fullDisplayName}",
                body: "The build ${currentBuild.fullDisplayName} failed.",
                to: "rodoracardales@gmail.com"
            )
        }
    }
}

