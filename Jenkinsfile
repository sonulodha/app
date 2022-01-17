pipeline {
    agent {
    label 'agent-pod'
}
    
    environment {
      dockerImage = ''
      registry = 'mitrasonu/nginx-alpine'
      registryCredential = 'docker-login'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/sonulodha/app.git']]])
            }
        }
        stage('Docker Build Image') {
                steps {
                    script {
                        dockerImage = docker.build registry
                    }
                }
            }
        stage('Docker Push Image'){
            steps {
                script {
                        docker.withRegistry('',registryCredential) {
                        dockerImage.push()
                    }
                }
            }
        }
   
        stage('Deploy App') {
            steps {
                script {
                        kubernetesDeploy(configs: "nginx-deployment.yaml", kubeconfigId: "connect-kubectl")
        }
      }
    }
    }
}
