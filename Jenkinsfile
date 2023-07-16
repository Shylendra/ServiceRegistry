pipeline {
    agent any
    tools{
        maven 'MAVEN_HOME'
    }
    stages{
        stage('Stage 1 - Checkout code'){
            steps{
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Shylendra/ServiceRegistry']])
            }
        }
        stage('Stage 2 - Build Maven'){
            steps{
                bat 'mvn clean install -Dspring.profiles.active=dev'
            }
        }
        stage('Stage 3 - Build docker image'){
            steps{
                bat 'docker build -t shylendra2015/service-registry:latest .'
            }
        }
        stage('Stage 4 - Login and Push image to Dockerhub'){
            steps{
              	withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                	bat "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                  bat 'docker push shylendra2015/service-registry:latest'
                }            
            }
        }
        /*
        stage('Stage 5 - Deploy to k8s'){
            steps{
            	withKubeConfig([credentialsId: 'shylendra.tm@gmail.com', serverUrl: 'https://127.0.0.1:52686']) {
            		bat 'C:/1_ShylendraLab/minikube-deploy/api-gateway-deployment.bat'
            	}
            }
        }
        */
        /*
        stage('Stage 5 - Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deployment.yml', kubeconfigId: 'KubernetesConfigPwd')
                    //kubernetesDeploy (configs: 'deployment.yml')
                }
            }
        }
        */
   }
    post {
        always {
            bat 'docker logout'
        }
    }
}