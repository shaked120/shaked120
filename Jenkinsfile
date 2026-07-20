pipeline {
    agent any

    environment {
        IMAGE_NAME = 'jenkins-flask-demo'
        CONTAINER_NAME = 'jenkins-flask-test'
    }

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code was downloaded from GitHub'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                    python3 -m venv venv
                    ./venv/bin/pip install --upgrade pip
                    ./venv/bin/pip install -r requirements.txt
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    ./venv/bin/pytest -v
                '''
            }
        }

        stage('Docker Build') {
            steps {
                sh '''
                    docker build \
                      -t ${IMAGE_NAME}:${BUILD_NUMBER} \
                      -t ${IMAGE_NAME}:latest \
                      .
                '''
            }
        }

        stage('Run Container') {
            steps {
                sh '''
                    docker rm -f ${CONTAINER_NAME} 2>/dev/null || true

                    docker run -d \
                      --name ${CONTAINER_NAME} \
                      -p 5000:5000 \
                      ${IMAGE_NAME}:${BUILD_NUMBER}

                    sleep 5
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    curl --fail http://localhost:5000/health
                '''
            }
        }
    }

    post {
        success {
            echo 'CI Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed — check the Console Output'
        }

        always {
            sh '''
                docker logs ${CONTAINER_NAME} 2>/dev/null || true
                docker rm -f ${CONTAINER_NAME} 2>/dev/null || true
            '''

            echo 'Pipeline cleanup completed'
        }
    }
}
