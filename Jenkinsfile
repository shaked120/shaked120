pipeline {
    agent any

    stages {

        stage('Checkout Test') {
            steps {
                echo 'Jenkins loaded this pipeline from Git'
            }
        }

        stage('System Info') {
            steps {
                sh '''
                    echo "Host: $(hostname)"
                    echo "User: $(whoami)"
                    uname -a
                '''
            }
        }

        stage('Health Check') {
            steps {
                sh '''
                    df -h /
                    free -h
                '''
            }
        }
    }

    post {
        success {
            echo 'Pipeline completed successfully'
        }

        failure {
            echo 'Pipeline failed - check the Console Output'
        }

        always {
            echo 'This runs whether the build succeeds or fails'
        }
    }
}
