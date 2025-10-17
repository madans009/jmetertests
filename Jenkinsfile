pipeline {
    agent any

    environment {
        JMETER_HOME = "/opt/apache-jmeter-5.6.3"  // Update this path as per your Jenkins setup
        JMETER_TEST = "tests/sample-test.jmx"     // Relative path to your JMeter script in repo
        REPORT_DIR  = "reports"
    }

    stages {

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/your-username/your-repo.git'
            }
        }

        stage('Run JMeter Test') {
            steps {
                echo "Running JMeter test: ${JMETER_TEST}"
                sh '''
                    mkdir -p ${REPORT_DIR}
                    ${JMETER_HOME}/bin/jmeter -n -t ${JMETER_TEST} -l ${REPORT_DIR}/results.jtl -e -o ${REPORT_DIR}/html
                '''
            }
        }

        stage('Publish HTML Report') {
            steps {
                publishHTML(target: [
                    allowMissing: false,
                    alwaysLinkToLastBuild: true,
                    keepAll: true,
                    reportDir: "${REPORT_DIR}/html",
                    reportFiles: 'index.html',
                    reportName: "JMeter HTML Report"
                ])
            }
        }
    }

    post {
        always {
            echo 'Cleaning workspace...'
            deleteDir()
        }
    }
}