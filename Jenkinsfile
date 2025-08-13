pipeline {
    agent any
    environment {
        DOCKER_HUB_REPO = "vikastiwari23/mlops"
        DOCKER_HUB_CREDENTIALS_ID = "jenkins_token"
        PATH = "/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin:${env.PATH}"
    }
    stages {
        stage('Checkout Github') {
            steps {
                echo 'Checking out code from GitHub...'
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'github_token', url: 'https://github.com/Vtiwari-talentica/mlops-demo.git']])
            }
        }
        stage('Check Docker Path') {
            steps {
                sh 'echo $PATH'
                sh 'which docker'
                sh 'docker --version'
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    echo 'Building Docker image...'
                    dockerImage = docker.build("${DOCKER_HUB_REPO}:latest")
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                script {
                    echo 'Pushing Docker image to DockerHub...'
                    sh 'docker login -u $DOCKER_HUB_REPO -p $DOCKER_HUB_CREDENTIALS_ID'
                    sh 'docker push ${DOCKER_HUB_REPO}:latest'
                }
            }
        }
    }
}

