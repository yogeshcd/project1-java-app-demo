pipeline {
    agent any
    stages{
        stage('sonar quality check'){

            steps {
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        // some block
                        sh 'mvn clean package sonar:sonar'
                    }
                }
            }
        }
    }
}