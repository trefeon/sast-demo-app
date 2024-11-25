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
                    // Use bash to run commands that use 'source'
                    sh '''
                    # Ensure bash is used to run the script
                    # Create and activate a virtual environment
                    python3 -m venv venv
                    bash -c "source venv/bin/activate"
                    pip install bandit
                    '''
                }
            }
        }
        stage('SAST Analysis') {
            steps {
                script {
                    // Run Bandit analysis and output results in XML format
                    sh '''
                    bash -c "source venv/bin/activate"
                    bandit -r . -f xml -o bandit_report.xml || true
                    '''
                }
            }
        }
        stage('Publish Bandit Results') {
            steps {
                // Publish the Bandit report results to Jenkins
                recordIssues(tools: [bandit(pattern: '**/bandit_report.xml')])
            }
        }
    }
}
