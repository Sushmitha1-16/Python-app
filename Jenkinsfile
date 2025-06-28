pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-creds')  // <-- your actual Jenkins credential ID
        IMAGE_NAME = 'santhu123456/pythonapp'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/betawins/Python-app.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t $IMAGE_NAME:$BUILD_NUMBER .'
            }
        }

        stage('Login to DockerHub') {
            steps {
                sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh 'docker push $IMAGE_NAME:$BUILD_NUMBER'
            }
        }
    }

    post {
        always {
            sh 'docker logout'
        }

        success {
            slackSend message: "✅ Docker Image pushed successfully: `${env.JOB_NAME}` #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        }

        failure {
            slackSend message: "❌ Docker Image push failed: `${env.JOB_NAME}` #${env.BUILD_NUMBER} (<${env.BUILD_URL}|Open>)"
        }
    }
}
