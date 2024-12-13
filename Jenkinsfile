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
                        sh 'mvn -Dmaven.test.failure.ignore=true clean package'
                    } else {
                        bat 'mvn -Dmaven.test.failure.ignore=true clean package'
                    }
                }
            }
        }
    }

    post {
        success {
            // Record the test results
            junit '**/target/surefire-reports/TEST-*.xml'
            // Archive the jar file
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
