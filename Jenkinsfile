pipeline {

    agent any

    stages {

        stage('Run tests') {
            steps {
                sh "docker-compose up"
            }
        }

        stage('Cleanup') {
            steps {
                sh "docker-compose down"
            }
        }

    }
}