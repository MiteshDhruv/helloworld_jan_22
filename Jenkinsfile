pipeline {
    agent any
    tools{
        maven 'M2_HOME'
    }
    environment {
    registry = '147450932347.dkr.ecr.us-east-2.amazonaws.com/my-docker-repo'
    registryCredential = 'AKIASEVGCVR5QMKVBTAL (registry_creds)'
    dockerimage = ''
  }
    stages {
        stage('Checkout'){
            steps{
                git branch: 'main', url: 'https://github.com/MiteshDhruv/helloworld_jan_22.git'
            }
        }
        stage('Code Build') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Build Image') {
            steps {
                script{
                    dockerImage = docker.build registry + ":$BUILD_NUMBER"
                } 
            }
        }
        stage('Deploy image') {
            steps{
                script{ 
                    docker.withRegistry("https://"+registry,"ecr:us-east-1:"+registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }  
    }
}
