pipeline {
    agent any
    environment {
      dockerImage = ''
      registry = 'arpitdoc/samplehttpd'
      registryCredential = 'dockerid'
    }
    stages {
        stage('Checkout') {
            steps {
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Sarpit/jenkins-docker.git']]])
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
                        kubernetesDeploy(configs: "deploy-httpd.yaml", kubeconfigId: "mykubeconfig")
        }
      }
    }
    }
}
