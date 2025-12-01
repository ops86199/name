pipeline {
    agent any

    environment {
        DOCKERHUB_USER = "ops86199"
        DOCKERHUB_PASS = credentials('dockerhub-pass')
        GITHUB_TOKEN   = credentials('github-token')
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'master',
                credentialsId: 'github-token',
                url: 'https://github.com/ops86199/name.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ops86199/ci-demo:latest .'
                }
            }
        }

        stage('Login to DockerHub') {
            steps {
                script {
                    sh "echo $DOCKERHUB_PASS | docker login -u $DOCKERHUB_USER --password-stdin"
                }
            }
        }

        stage('Push Image') {
            steps {
                script {
                    sh 'docker push ops86199/ci-demo:latest'
                }
            }
        }

        stage('Run Container') {
            steps {
                script {
                    sh 'docker rm -f ci-demo || true'
                    sh 'docker run -d --name ci-demo -p 3000:3000 ops86199/ci-demo:latest'
                }
            }
        }
    }
}
