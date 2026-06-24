pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Clean Previous Results') {
            steps {
                bat '''
                if exist "%WORKSPACE%\\TestResults.jtl" del /F /Q "%WORKSPACE%\\TestResults.jtl"
                if exist "%WORKSPACE%\\HTMLReport" rmdir /S /Q "%WORKSPACE%\\HTMLReport"
                '''
            }
        }

        stage('Run JMeter') {
            steps {
                bat '''
                C:\\Jmeter\\bin\\jmeter.bat -n ^
                -t "%WORKSPACE%\\JpetStoreJmeterFinal.jmx" ^
                -l "%WORKSPACE%\\TestResults.jtl" ^
                -e ^
                -o "%WORKSPACE%\\HTMLReport"
                '''
            }
        }
    }

    post {

        always {

            archiveArtifacts artifacts: 'TestResults.jtl', fingerprint: true

            publishHTML(target: [
                reportDir: 'HTMLReport',
                reportFiles: 'index.html',
                reportName: 'JMeter Performance Report',
                keepAll: true,
                alwaysLinkToLastBuild: true,
                allowMissing: false
            ])

        }
    }
}
