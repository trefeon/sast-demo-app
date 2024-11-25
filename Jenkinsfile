pipeline {
    agent any
    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Install Dependencies') {
            steps {
                script {
                    // Install Bandit in a virtual environment
                    sh '''
                    python3 -m venv venv
                    source venv/bin/activate
                    pip install bandit
                    '''
                }
            }
        }
        stage('SAST Analysis') {
            steps {
                script {
                    // Run Bandit on the repository and output results in XML format
                    sh '''
                    source venv/bin/activate
                    bandit -r . -f xml -o bandit_report.xml || true
                    '''
                }
            }
        }
        stage('Publish Bandit Results') {
            steps {
                // Publish Bandit report as part of the build results
                recordIssues(tools: [bandit(pattern: '**/bandit_report.xml')])
            }
        }
    }
}
