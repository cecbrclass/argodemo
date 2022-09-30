pipeline {
    agent any
    stages {
        stage ('Build Image'){
            sh 'sed -i "s/DEMO/${env.BUILD_ID}/g" ./app/index.html'
            steps {
                echo 'Starting Pipeline'
                script {
                    dockerapp = docker.build("ezmeralreg.cec.dev.br/cecbr/nginx:${env.BUILD_ID}", '-f ./src/Dockerfile ./src')
                }
            }
        }
        stage ('Push Image'){
            steps {
                 script {
                    docker.withRegistry('https://ezmeralreg.cec.dev.br', 'cecbr'){
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
