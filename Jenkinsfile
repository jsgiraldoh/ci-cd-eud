pipeline {
    agent {
        label 'docker'
    }
    stages {
        stage('Source') {
            steps {
                git 'https://github.com/jsgiraldoh/unir-test.git'
            }
        }
        stage('Build') {
            steps {
                echo 'Build!'
                docker build --tag img-php .
            }
        }
        stage('Test') {
            steps {
                echo 'Test!'
                docker scan img-php
            }
        }
        stage('Push') {
            steps {
                echo 'Push!'
                docker tag img-php jsgiraldoh/img-php-eud
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploy!'
                docker run -d -p 80:80 --name my-running-app jsgiraldoh/img-php-eud
            }
        }
    }
    post {
        sucess{
            echo "Good Job!"
        }
        always{
            echo "Always Notification!"
            cleanWs()
        }
        failure {
            echo "Failure Notification!"
            echo "${currentBuild.currentResult}: Job ${env.JOB_NAME}\nBuild Number: ${env.BUILD_NUMBER}\nMore Info can be found here: ${env.BUILD_URL}"
            cleanWs()
        }
    }
