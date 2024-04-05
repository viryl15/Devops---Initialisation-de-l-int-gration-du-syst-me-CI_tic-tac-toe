pipeline {
    agent any
    tools {
        nodejs 'nodejs20'
    }
    stages {
        stage('Install Dependencies') {
            steps {
                echo 'Installing dependencies...'
                sh  'npm install'
            }
        }
        stage('Test') {
            steps {
                echo 'Functional Testing...'
                sh 'CI=true npm test -- --coverage'
            }
            post {
                    always {
                        clover(cloverReportDir: 'coverage', cloverReportFileName: 'clover.xml',
                        // optional, default is: method=70, conditional=80, statement=80
                        healthyTarget: [methodCoverage: 70, conditionalCoverage: 80, statementCoverage: 80],
                        // optional, default is none
                        unhealthyTarget: [methodCoverage: 50, conditionalCoverage: 50, statementCoverage: 50],
                        // optional, default is none
                        failingTarget: [methodCoverage: 0, conditionalCoverage: 0, statementCoverage: 0]
                        )
                    }
                }
        }
        stage('Build') {
            steps {
                echo 'Building the application...'
                sh "npm run build"
            }
            post {
                always {
                    archiveArtifacts artifacts: 'build/**/*.*', onlyIfSuccessful: true
                }
            }
        }
    }
    post {
        always {
            echo "Pipeline completed. Cleaning up..."
            sh "rm -rf node_modules"
        }
    }
}
