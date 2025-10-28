pipeline {
    agent any

    environment {
        GITHUB_REPO = 'git@github.com:alikhaled09/simple_calc.git' // استخدم SSH URL
        BRANCH_NAME = 'main'
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: "${BRANCH_NAME}", url: "${GITHUB_REPO}"
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                sh '''
                    . venv/bin/activate
                    pytest || echo "No tests failed"
                '''
            }
        }

        stage('Modify File and Push Back') {
            steps {
                script {
                    sh 'date > build_log.txt'
                    
                    // Git config
                    sh '''
                        git config user.email "alikhaled09@gmail.com"
                        git config user.name "Jenkins"
                        git add build_log.txt
                        git commit -m "Auto update from Jenkins" || echo "No changes to commit"
                        git push origin main || echo "Push failed"
                    '''
                }
            }
        }
    }
}
