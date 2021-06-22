pipeline {
    agent any 

    stages {
        stage('Checkout Source') {
            steps {
                git url: 'https://github.com/andre-mf/pedelogo-catalogo.git', branch: 'main'
            }
        }
       stage('Build Image') {
            steps {
                script {
                    dockerapp = docker.build("andremf/pedelogo-catalogo:${env.BUILD_ID}",
                    '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                        docker.withRegistry('http://registry.hub.docker.com', 'dockerhub') {   
                        dockerapp.push('latest')     
                        dockerapp.push("${env.BUILD_ID}")  
                    }   
                }
            }
        }      

        stage('Deploy kubernetes') {
            agent {
                kubernetes {
                    cloud 'kubernetes'
                }
            }

            steps {
                    kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
            }
        }                    
    }
}   