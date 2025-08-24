
pipeline {
    agent any
    
    tools {
        maven 'maven3'    
    }
    
    environment {
        SCANNER_HOME = tool 'sonar-scanner'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/gopal5raut/Boardgame-main.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Package') {
            steps {
                sh 'mvn package'
            }
        }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('sonarqube') {
                    sh '''
                        $SCANNER_HOME/bin/sonar-scanner \
                        -Dsonar.projectName=BoardGame \
                        -Dsonar.projectKey=BoardGame \
                        -Dsonar.java.binaries=target
                    '''
                }
            }
        }
        stage('Quality-gate-check') {
            steps {
           timeout(time: 15, unit: 'minutes') {
            waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'   
             }
            }
        }
    }
}
