pipeline {
    agent any
    environment {
        registry = "yaswanthlingamaneni/swe645-hw2"
        registryCredential = 'dockerhub'
    }
    stages {
        stage('Intialization') {
            steps {
                script {
                    env.date = new Date().format("yyyyMMdd-HHmmss")
                }
            }
        }
        stage('Building') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        def image = docker.build("${registry}:${env.date}", ". --no-cache --platform=linux/amd64")
                    }
                }
            }
        }
        stage('Pushing') {
            steps {
                script {
                    docker.withRegistry('', registryCredential) {
                        def image = docker.image("${registry}:${env.date}")
                        image.push()
                    }
                }
            }
        }
        stage('Deploying') {
            steps {
                script {
                    sh "kubectl set image deployment/nodeport container-0=${registry}:${env.date}"
                    sh "kubectl set image deployment/loadbalancer container-0=${registry}:${env.date}"
                }
            }
        }
    }
    post {
        always {
            sh 'docker logout'
        }
    }
}