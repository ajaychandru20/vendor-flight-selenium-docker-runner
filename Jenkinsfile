pipeline{
    agent {
        label 'ec2-fleet'
    }
    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the Browser', name: 'BROWSER'
    }
    stages{
        stage('Docker Compose Up Grid'){
            steps{
                sh "docker compose -f grid.yaml up --scale ${params.BROWSER}=1 -d"
            }
        }
        stage('Run Test Suites'){
            steps{
                sh "docker compose -f test-suite.yaml up --abort-on-container-exit"
                script {     
                    if(fileExists('output/flight-booking/testng-failed.xml') || fileExists('output/vendor-portal/testng-failed.xml')){
                        error('failed tests found')
                    }
                }
                sleep time: 5, unit: 'SECONDS'
            }
        }
    }
    post{
        always{
            sh "docker compose -f test-suite.yaml logs --no-color > test_suite_logs.txt"
            archiveArtifacts artifacts: 'test_suite_logs.txt', followSymlinks: false
            sh "docker compose -f test-suite.yaml down"
            sh "docker compose -f grid.yaml down"
            archiveArtifacts artifacts: 'output/flight-booking/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}