pipeline {
    agent any

    environment {
        APP_NAME = "dogs-management-system"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/s566466div/bathalaprojectMilestone05.git'
            }
        }

        stage('Build and Create Docker Image') {
            steps {
                sh 'mvn clean package spring-boot:build-image -Dspring-boot.build-image.imageName=${APP_NAME}:${DOCKER_TAG}'
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    sh """
                        if [ "$(docker ps -q -f name=${APP_NAME})" ]; then
                            docker stop ${APP_NAME}
                        fi
                        if [ "$(docker ps -a -q -f name=${APP_NAME})" ]; then
                            docker rm ${APP_NAME}
                        fi
                    """
                }
            }
        }

        stage('Run New Container') {
            steps {
                sh "docker run -d --name ${APP_NAME} -p 8080:8080 ${APP_NAME}:${DOCKER_TAG}"
            }
        }
    }

    post {
        success {
            echo "✅ Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed! Check logs."
        }
    }
}
