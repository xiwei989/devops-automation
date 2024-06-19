pipeline{
    agent any
    tools{
        maven 'maven 3.9.8'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/xiwei989/devops-automation']])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                echo 'Starting to build docker image'
                script{
                    sh 'docker build -t xiwei989/devops-integration:jenkins .'
                }
                echo 'Build docker image done.'
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhub-pwd', variable: 'dockerhubpwd')]) {
                        sh 'docker login -u xiwei989 -p ${dockerhubpwd}'
                    }
                    sh 'docker push xiwei989/devops-integration:jenkins'
                }
            }
        }
    }
}