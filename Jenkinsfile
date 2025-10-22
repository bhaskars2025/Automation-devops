pipeline {
    agent any

    environment {
        // Replace with actual repo URLs
        GITHUB_REPO = 'https://github.com/bhaskars2025/Automation-devops.git'
        BITBUCKET_REPO = 'git@bitbucket.org:your-org/your-repo.git'
    }

 

    options {
        skipDefaultCheckout()
    }

    stages {
        stage('Validate Branch') {
            when {
                anyOf {
                    branch 'main'
                    branch 'Develop'
                    
                }
            }
            steps {
                echo "Triggered by valid branch: ${env.BRANCH_NAME}"
            }
        }

        stage('Checkout from GitHub') {
            when {
                anyOf {
                    branch 'main'
                    branch 'Develop'
                    
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'bhaskar2025', usernameVariable: 'GIT_USER', passwordVariable: 'GIT_PAT')]) {
                    sh '''
                        git config --global user.name "$bhaskar"
                        git config --global user.email "bhaskar@ci.local"

                        git clone --branch ${BRANCH_NAME} https://${GIT_USER}:${GIT_PAT}@${GITHUB_REPO} repo
                    '''
                }
            }
        }

        stage('Force Push to Bitbucket') {
            when {
                anyOf {
                    branch 'main'
                    branch 'Develop'
                    
                }
            }
            steps {
                sshagent(['bitbucket-ssh-key']) {
                    dir('repo') {
                        sh '''
                            git remote add bitbucket ${BITBUCKET_REPO}
                            git push bitbucket ${BRANCH_NAME}:${BRANCH_NAME} --force
                        '''
                    }
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed for branch ${env.BRANCH_NAME}"
        }
        success {
            echo "Successfully synced ${env.BRANCH_NAME} to Bitbucket"
        }
    }
}
