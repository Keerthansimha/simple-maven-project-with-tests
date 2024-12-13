pipeline {
    agent any

    tools {
        maven 'maven'
        jdk 'java'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', 
                    branches: [[name: '*/main']], 
                    extensions: [], 
                    userRemoteConfigs: [[credentialsId: 'github-access', url: 'https://github.com/Keerthansimha/simple-maven-project-with-tests.git']]
                ])
            }
        }
        stage('Build') {
            steps {
                script {
                    // Automatically detect the OS and use the appropriate command
                    if (isUnix()) {
                        sh 'mvn clean install' // Run the complete lifecycle, including tests
                    } else {
                        bat 'mvn clean install' // Run the complete lifecycle, including tests
                    }
                }
            }
        }
    }

    post {
        success {
            // Record the test results if the build is successful
            junit '**/target/surefire-reports/TEST-*.xml'
            // Archive the jar file as an artifact
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
        failure {
            // Handle failure case (e.g., notify or fail the build in a specific way)
            echo 'Build or tests failed!'
        }
    }
}
