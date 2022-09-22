pipeline {
    agent any
    stages {
        stage ('Inicial'){
            steps {
                echo 'Starting Pipeline'
                script {
                    dockerapp = docker.build("docker.io/nginx", '-f ./src/Dockerfile ./src')
                }
            }
        }
    }
}