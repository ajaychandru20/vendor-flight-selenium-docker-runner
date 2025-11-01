pipeline{
    agent any
    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the Browser', name: 'BROWSER'
    }
    stages{
        stage('Docker Compose Up Grid'){
            steps{
                sh "docker compose -f grid.yaml up --scale ${params.BROWSER}=2 -d"
            }
        }
        stage('Run Test Suites'){
            steps{
                sh "docker compose -f test-suite.yaml up --abort-on-container-exit"
            }
        }
    }
    post{
        always{
            sh "docker compose -f test-suite.yaml down"
            sh "docker compose -f grid.yaml down"
            archiveArtifacts artifacts: 'output/flight-booking/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}