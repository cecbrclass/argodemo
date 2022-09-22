pipeline {
    agent any
    stages {
        stage ('Inicial'){
            steps {
                echo 'Starting Pipeline'
                script {
                    dockerapp = docker.build("ezmeralreg.cec.dev.br/cecbr/nginx", '-f .src/Dockerfile ./src')
                }
            }
        }
    }
}