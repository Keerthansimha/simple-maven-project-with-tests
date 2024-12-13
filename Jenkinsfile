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
        stage('Validate') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn validate'
                    } else {
                        bat 'mvn validate'
                    }
                }
            }
        }
        stage('Compile') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn compile'
                    } else {
                        bat 'mvn compile'
                    }
                }
            }
        }
        stage('Test') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn test'
                    } else {
                        bat 'mvn test'
                    }
                }
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
        stage('Package') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn package'
                    } else {
                        bat 'mvn package'
                    }
                }
            }
        }
        stage('Verify') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn verify'
                    } else {
                        bat 'mvn verify'
                    }
                }
            }
        }
        stage('Install') {
            steps {
                script {
                    if (isUnix()) {
                        sh 'mvn install'
                    } else {
                        bat 'mvn install'
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
