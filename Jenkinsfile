pipeline{
    agent any
    stages{
        stage('Docker Compose Up Grid'){
            steps{
                sh "docker compose -f grid.yaml up -d"
            }
        }
        stage('Run Test Suites'){
            steps{
                sh "docker compose -f test-suites.yaml up --abort-on-container-exit"
            }
        }
    }
    post{
        always{
            sh "docker compose -f test-suites.yaml down"
            sh "docker compose -f grid.yaml down"
        }
    }
}