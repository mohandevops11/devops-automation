pipeline {
    
    agent any
    tools{
        maven 'maven_3.8.6'
    }
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/mohandevops11/devops-automation.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t mohandevops111/devops-integration .'
                }
            }
        }
        stage('Push image to Hub'){
            steps{
                script{
                   withCredentials([string(credentialsId: 'mohandevops111', variable: 'dockerhubpwd')]) {
                   sh 'docker login -u mohandevops111 -p ${dockerhubpwd}'

}
                   sh 'docker push mohandevops111/devops-integration'
                }
            }
        }
        stage('Deploy to k8s'){
            steps{
                script{
                    kubectl apply -f deploymentservice.yaml
                    kubernetesDeploy (configs: 'deploymentservice.yaml')
                }
            }
        }
    }
}
