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
        stage('Stage 5 - Deploy to k8s'){
            steps{
            	kubeconfig(credentialsId: 'KubernetesConfigPwdCLI', caCertificate: '''MIIDBjCCAe6gAwIBAgIBATANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwptaW5p
a3ViZUNBMB4XDTIzMDYyODA4NTQxM1oXDTMzMDYyNjA4NTQxM1owFTETMBEGA1UE
AxMKbWluaWt1YmVDQTCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAMfY
lNW1UvRJDKLgxC7Srj3sLnyWd6tUB8VxQOvzIx4geA7Fd0ekEDFSTZ7tnVP5YFtt
PKUugZgZgtJJJJx2nDsWqutgq+OTiGHzPcn9dVlVWIIPlU7Ix3cB1RZWzbDJLmkl
G/wKe84oKqCKgOKTlfUNilRI0j0GY99tQK+aAUK2EPoMQboaiWjrGnulzj8CLL4y
bcDyL2GZfiDN/Dk+oiPaNp0wRPQTX/LJBnLfKEdcSIy7s1UiGS+dILG0xZGl/sZW
t/48huaBe1YEispzkQuJekUV+1cJm8/+TiYsnWBgezOOW+ISH+dLZ8FLKcoOHsDG
+hKWL85KEN7dy34P0p8CAwEAAaNhMF8wDgYDVR0PAQH/BAQDAgKkMB0GA1UdJQQW
MBQGCCsGAQUFBwMCBggrBgEFBQcDATAPBgNVHRMBAf8EBTADAQH/MB0GA1UdDgQW
BBQKVTvzjjyi9X8fn04MHF4QMdW89TANBgkqhkiG9w0BAQsFAAOCAQEAlfviN3JD
Hbcb4l79kajlf9rsOzNPDBfPSYvMyBAL1uJnRnEvDSNXSI7LRuSpzU+BrDHmFvHf
sN9k9Io2FEgMHTVg/rXiEU3GTrJthT3xyQrzs3xpgOrzFBVQMaSDQE8jYfYhn/Pa
NzEs0Y6yAjR3mRvecKzO5PtLC31LE82YmwxDpWXlQn1vso7ulcdRTYRVq7ZZn0Tx
PBGL9o/LqgBrZH7sL7Hk+3WhPeq7YVDW/zqqhoTnUL6OVLZwZBRvoTduu4Oqal0P
+EH63q8ctAlXGBVrESUi9CLZifzb9z6mBAoFTRPUKFeU3/z4iVyeITyApSB4sN5n
4ZAUNyDl0Q777A==''', serverUrl: 'https://127.0.0.1:52686') {
            		bat 'C:/1_ShylendraLab/minikube-deploy/api-gateway-deployment.bat'
            	}
                
            }
        }
        /*
        stage('Stage 5 - Deploy to k8s'){
            steps{
                script{
                    kubernetesDeploy (configs: 'deployment.yml', kubeconfigId: 'KubernetesConfigPwd')
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