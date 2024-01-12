pipeline {

    agent any

    parameters {
        choice choices: ['chrome', 'firefox'], description: 'Select the browser', name: 'BROWSER'
    }

    stages {

        stage('Start selenium grid') {
            steps {
                sh "docker-compose -f grid.yaml up --scale ${params.BROWSER}=1 -d"
            }
        }

        stage('Execute test suites') {
            steps {
                sh "docker-compose -f test-suites.yaml up --pull=always"
                script {
                    if (fileExists('output/vendor-portal/testng-failed.xml') || fileExists("output/flight-reservation/testng-failed.xml")) {
                        error("test failure occurred!")
                    }
                }
            }
        }
    }
    post {
        always {
            sh "docker-compose -f grid.yaml down"
            sh "docker-compose -f test-suites.yaml down"
            archiveArtifacts artifacts: 'output/flight-reservation/emailable-report.html', followSymlinks: false
            archiveArtifacts artifacts: 'output/vendor-portal/emailable-report.html', followSymlinks: false
        }
    }
}