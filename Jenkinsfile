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


        stage('Install Dependencies') {

            steps {

                echo "Installing Node dependencies"

                dir('backend') {

                    sh 'npm install'

                }

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

                echo "Stopping old container"

                sh '''
                docker stop $CONTAINER_NAME || true
                docker rm $CONTAINER_NAME || true
                '''

            }

        }


        stage('Run New Container') {

            steps {

                echo "Starting new container"

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

                echo "Checking running container"

                sh 'docker ps'

            }

        }

    }


    post {


        success {

            echo "================================"
            echo "Deployment Successful!"
            echo "Attendance System is Running"
            echo "================================"

        }


        failure {

            echo "================================"
            echo "Deployment Failed!"
            echo "Check Console Output"
            echo "================================"

        }


    }

}