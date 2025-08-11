pipeline {
    agent any

    environment {
        APP_NAME = "my-springboot-app"
        DOCKER_TAG = "latest"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Build with Maven') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${APP_NAME}:${DOCKER_TAG} ."
            }
        }

        stage('Stop Old Container') {
            steps {
                script {
                    sh """
                        if [ \$(docker ps -q -f name=${APP_NAME}) ]; then
                            docker stop ${APP_NAME}
                        fi
                        if [ \$(docker ps -a -q -f name=${APP_NAME}) ]; then
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
            echo "✅ Application deployed successfully on local Docker!"
        }
        failure {
            echo "❌ Pipeline failed. Check the logs!"
        }
    }
}
