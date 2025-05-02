pipeline {
    agent { label 'docker' }

    environment {
        IMAGE_NAME = 'chervinskyds/step2-app'
    }

    stages {
        stage('Pull code') {
            steps {
                git branch: 'main', url: 'https://github.com/chervinskyds/step2-app.git'
            }
        }

        stage('Build image') {
            steps {
                sh 'docker build -t $IMAGE_NAME .'
            }
        }

        stage('Run tests') {
            steps {
                sh '''
                    docker run --rm $IMAGE_NAME npm test || {
                        echo "Tests failed"
                        exit 1
                    }
                '''
            }
        }

        stage('Push image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh '''
                        echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin
                        docker push $IMAGE_NAME
                    '''
                }
            }
        }
    }

    post {
        failure {
            echo 'Pipeline failed.'
        }
        success {
            echo 'Pipeline completed successfully.'
        }
    }
}
