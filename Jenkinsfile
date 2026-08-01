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

                dir('backend') {

                    bat 'npm install'

                }

            }

        }


        stage('Build Docker Image') {

            steps {

                dir('backend') {

                    bat 'docker build -t %IMAGE_NAME% .'

                }

            }

        }


        stage('Stop Old Container') {

            steps {

                bat '''
                docker stop %CONTAINER_NAME% || exit 0
                docker rm %CONTAINER_NAME% || exit 0
                '''

            }

        }


        stage('Run Docker Container') {

            steps {

                bat '''
                docker run -d -p 5000:5000 --name %CONTAINER_NAME% %IMAGE_NAME%
                '''

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