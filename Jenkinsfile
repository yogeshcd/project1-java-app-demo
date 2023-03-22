pipeline {
    agent any
    environment {
        VERSION= "{$env.BUILD_ID}"
    }

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
                    withCredentials([string(credentialsId: 'nexu_passwd', variable: 'nexus-cred')]) {
                        // some block
                        //docker build -t test-app .
                    sh "ls -l"
                    sh '''
                    docker build -t 192.168.56.1:8083/springapp:${VERSION} .
                    docder login -u admin -p $nexus-cred 192.168.56.1:8083
                    docker push 192.168.56.1:8083/springapp:${VERSION}
                    docker rmi 192.168.56.1:8083/springapp:${VERSION}
                    '''                        
                    }

                }
            }
        }
    }
}
