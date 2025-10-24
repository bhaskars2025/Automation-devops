pipeline {
    agent any

    environment {
        GITHUB_CREDS = credentials('bhaskar2025')
        BITBUCKET_CREDS = credentials('bhaskar-bitbucket-admin')
        GITHUB_REPO = 'https://github.com/bhaskars2025/Automation-devops.git'
        BITBUCKET_REPO = 'https://bhaskar-bitbucket-admin@bitbucket.org/bhaskar-bitbucket/automation-devops.git'
    }

    stages {
        stage('Clone from GitHub') {
            steps {
                git url: "${GITHUB_REPO}", credentialsId: "${GITHUB_CREDS}"
            }
        }

        stage('Push to Bitbucket') {
            steps {
                sh """
                    git remote add bitbucket ${BITBUCKET_REPO}
                    git push bitbucket HEAD:main
                """
            }
        }
    }
}
