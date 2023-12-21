pipeline {
    agent any

    environment {
        SNYK_TOKEN = credentials('SNYK_TOKEN')
        SNYK_ORG = credentials('SNYK_ORG_GRADLE_CLI')
    }

    stages {
        stage('Download Snyk CLI, snyk-delta') {
            steps {
                sh '''
                    curl -Lo ./snyk "https://static.snyk.io/cli/latest/snyk-linux"
                    curl -Lo ./snyk-delta "https://github.com/snyk-tech-services/snyk-delta/releases/download/v1.12.0/snyk-delta-linux"
                    chmod +x snyk
                    chmod +x snyk-delta
                    mv ./snyk /usr/local/bin/
                    mv ./snyk-delta /usr/local/bin/
                '''
            }
        }

        stage('Snyk Test using Snyk CLI') {
            steps {
                sh '''
                    cd app
                    snyk test --severity-threshold=high --json --print-deps | snyk-delta  --baselineOrg $SNYK_ORG
                    cd ..
                '''
            }
        }
    }
}