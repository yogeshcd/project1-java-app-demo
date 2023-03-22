pipeline {
    agent any
    stages{
        stage('sonar quality check'){
            agent{
                docker {
                    image 'maven'
                }
            }
            steps {
                // script {
                //     withSonarQubeEnv(credentialsId: 'sonar-token') {
                //         // some block
                //         sh 'mvn clean package sonar:sonar'
                //     }
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        // some block
                        sh 'mvn clean package sonar:sonar'
                    }
                    
                
                }
            }
        }
        stage(" Quality gate") {
            steps {
                script {
                    waitForQualityGate abortPipeline: false, credentialsId: 'sonar-token'
                }
            }
        }
        stage("docker build and push to nexus") {
            steps {
                script {
                    sh "ls -l"
                    sh "docker build -t test-app ."
                }
            }
        }
    }
}
