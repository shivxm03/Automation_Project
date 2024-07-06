pipeline {
    agent any
    tools{
        maven 'Apache Maven 3.9.8'
    }
    
    stages{
        stage('Build Maven'){
            steps{
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/shivxm03/Automation_Project.git']]])
                sh 'mvn clean install'
            }
        }
        stage('Build docker image'){
            steps{
                script{
                    sh 'docker build -t shivamp03/kubernetes .'
                }
            }
        }
          stage('Push image to hub'){
            steps{
                script{
                    withCredentials([string(credentialsId: 'dockerhubpwd', variable: 'dockerhubinfo')]) {
                    sh 'docker login -u shivamp03 -p ${dockerhubinfo}'
                    sh 'docker push shivamp03/kubernetes'
                }
            }
        }
        stage('Deploy to K8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deploymentservice.yaml',kubeconfigId: 'kubeconfig')
                }
            }
        }
    
    }    
}
