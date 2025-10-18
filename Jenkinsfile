pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                git url: 'https://github.com/madans009/jmetertests.git', branch: 'main'
            }
        }
        stage('Run JMeter Test') {
            agent {
                docker { image 'justb4/jmeter:5.7.2' }
            }
            steps {
                sh 'jmeter -n -t testplans/testplan.jmx -l result.jtl -e -o report'
            }
        }
        stage('Archive Report') {
            steps {
                archiveArtifacts artifacts: 'report/**', fingerprint: true
            }
        }
    }
}