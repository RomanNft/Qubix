pipeline {
    agent any
    
    stages {
        stage('Clone repository') {
            steps {
                git branch: 'main', url: 'https://github.com/RomanNft/qwqaz'
            }
        }

        stage('Build Docker Containers') {
            steps {
                sh 'docker-compose build'
            }
        }

        stage('List Docker Images') {
            steps {
                sh 'docker images'
            }
        }

        stage('Run Containers') {
            steps {
                sh 'docker-compose up -d'
            }
        }

        stage('Push Docker Images') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'my_service_', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {
                    sh """
                    echo $DOCKER_PASS | docker login -u $DOCKER_USER --password-stdin
                    docker-compose push --verbose
                    """
                }
            }
        }

        stage('Clean Up') {
            steps {
                sh 'docker-compose down'
            }
        }
    }
}
