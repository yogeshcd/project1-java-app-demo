pipeline {
    agent any
    stages{
        stage('sonar quality check'){
            // agent{
            //     docker {
            //         image 'maven'
            //     }
            // }
            steps {
                // script {
                //     withSonarQubeEnv(credentialsId: 'sonar-token') {
                //         // some block
                //         sh 'mvn clean package sonar:sonar'
                //     }
                script {
                    withSonarQubeEnv(credentialsId: 'sonar-token') {
                        // some block
                        echo "step1"
                    }
                    
                
                }
            }
        }
    }
}