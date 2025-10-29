pipeline{
    agent any
    stages{
        stage('Docker Compose Up'){
            steps{
                sh 'docker compose up'
            }
        }
        stage('Docker Compose Down'){
            steps{
                sh 'docker compose down'
            }
        }
    }
}