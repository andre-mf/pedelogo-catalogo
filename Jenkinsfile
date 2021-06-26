pipeline {
    agent any

    stages {

        stage('Get source') {
            steps {
                git url: 'https://github.com/andre-mf/pedelogo-catalogo', branch: 'main'
            }
        }

        // stage('Docker Build') {
        //     steps {
        //         script {
        //             dockerapp = docker.build("andremf/pedelogo-catalogo:${env.BUILD_ID}",
        //                 '-f ./src/PedeLogo.Catalogo.Api/Dockerfile .')
        //         }
        //     }
        // }

        // stage('Docker Push Image') {
        //     steps {
        //         script {
        //             docker.withRegistry('https://registry.hub.docker.com', 'dockerhub') {
        //                 dockerapp.push('latest')
        //                 dockerapp.push("${env.BUILD_ID}")
        //             }
        //         }
        //     }
        // }

        stage('Deploy Kubernetes') {
            agent {
                kubernetes {
                    cloud 'kubernetes'
                }
            }

            steps {
                // script {
                //     sh("uname -a")
                //     sh("ping -c 10 8.8.8.8")
                // }
                // curl 192.168.1.2:37559
                // ping 127.0.0.1
                // curl 127.0.0.1:8085
                kubernetesDeploy(configs: '**/k8s/**', kubeconfigId: 'kubeconfig')
            }
        }
    }
}