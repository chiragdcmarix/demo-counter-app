pipeline {
    agent any
    
    stages {
        stage('Git Checkout') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/chiragdcmarix/demo-counter-app.git'
                }
            }
        }

        stage('UNIT Testing') {
            steps {
                script {
                    sh 'mvn test'
                }
            }
        }

        stage('Integration Testing') {
            steps {
                script {
                    sh 'mvn verify -DskipUnitTests'
                }
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    sh 'mvn clean install'
                }
            }
        }

        stage('Static Code Analysis') {
            steps {
                script {
                    withSonarQubeEnv('sonar-server') {
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }

        stage('Quality Gate Status') {
            steps {
                script {
                    waitForQualityGate abortPipeline: false
                }
            }
        }

        stage('Upload WAR File to Nexus') {
            steps {
                script {
                    nexusArtifactUploader(
                        artifacts: [
                            [artifactId: 'springboot', classifier: '', file: 'target/Uber.jar', type: 'jar']
                        ], 
                        credentialsId: 'nexus-auth', 
                        groupId: 'com.example', 
                        nexusUrl: '203.109.113.163:8081', 
                        nexusVersion: 'nexus3', 
                        protocol: 'http', 
                        repository: 'maven-demo-releases', 
                        version: '1.0.0'
                    )
                }
            }
        }
    }
}
