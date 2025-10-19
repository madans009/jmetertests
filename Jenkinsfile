pipeline {
    agent any

    environment {
        TESTPLAN = "${WORKSPACE}/testplans/testplan.jmx"
        RESULTS = "${WORKSPACE}/testplans/result.jtl"
        REPORT = "${WORKSPACE}/testplans/report"
    }

    stages {
        stage('Clean Previous Results') {
            steps {
                sh '''
                    rm -rf $RESULTS
                    rm -rf $REPORT
                    mkdir -p $REPORT
                '''
            }
        }

        stage('Run JMeter Test') {
            steps {
                sh '''
                    /opt/jmeter/bin/jmeter -n -t $TESTPLAN -l $RESULTS -e -o $REPORT
                '''
            }
        }

        stage('Publish Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: 'testplans/report',
                    reportFiles: 'index.html',
                    reportName: 'JMeter Test Report'
                ])
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'testplans/result.jtl', allowEmptyArchive: true
        }
    }
}
