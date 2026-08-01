pipeline {

    agent any

    environment {
        IMAGE_NAME = "attendance-app"
        CONTAINER_NAME = "attendance"
    }

    stages {

        stage('Checkout') {
            steps {
                echo "Pulling code from GitHub"
                checkout scm
            }
        }


        stage('Build Docker Image') {
            steps {
                echo "Building Docker Image"

                dir('backend') {
                    sh 'docker build -t $IMAGE_NAME .'
                }
            }
        }


        stage('Stop Existing Container') {
            steps {

                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''

            }
        }


        stage('Run New Container') {
            steps {

                sh '''
                docker run -d \
                -p 5000:5000 \
                --name $CONTAINER_NAME \
                $IMAGE_NAME
                '''

            }
        }


        stage('Check Container') {
            steps {

                sh 'docker ps'

            }
        }

    }


    post {

        success {
            echo "Deployment Successful!"
        }

        failure {
            echo "Deployment Failed!"
        }

    }

}