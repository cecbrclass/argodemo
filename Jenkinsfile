pipeline {
    agent any
    stages {
        stage ('Build Image'){
            steps {
                echo 'Starting Pipeline Demo'
                 script {
                    dockerapp = docker.build("harbor.cec.dev.br/ezmeral/nginx:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage ('Push Image'){
            steps {
                 script {
                    docker.withRegistry('https://harbor.cec.dev.br', 'ezmeral'){
                    dockerapp.push('latest')
                    dockerapp.push("${env.BUILD_ID}")
                    }
                }
            }
        }
        stage ('Deploy Kubernetes'){
            environment {
                tag_version = "${env.BUILD_ID}"
            }
            steps {
                 withKubeConfig([credentialsId: 'kubeconfigfile']){
                    sh 'sed -i "s/{{tag}}/$tag_version/g" ./nginx/deploy.yaml'
                    sh 'kubectl apply -f ./nginx/deploy.yaml -n tdemo'
                }
            }
        }
    }
}
