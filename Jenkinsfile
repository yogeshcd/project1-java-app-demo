pipeline {
    agent any
    environment {
        VERSION= "${env.BUILD_ID}"
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
                    docker login -u admin -p admin 192.168.56.1:8083
                    docker push 192.168.56.1:8083/springapp:${VERSION}
                    docker rmi 192.168.56.1:8083/springapp:${VERSION}
                    '''                        
                    }
                }
            }
        }
        stage("Upload helm file to nexus repo"){
            agent {
                docker { image "alpine/k8s:1.24.12" }
            }
            steps {
                script {
                    sh '''
                        helm version
                        cd kubernetes
                        helm package myapp
                        curl -u admin:admin 192.168.56.1:8081/repository/myhelmrepo/ --upload-file myapp-0.2.0.tgz
                        '''
                }
                
            }
        }
    }
}
