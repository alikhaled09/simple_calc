pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/alikhaled09/simple_calc.git'
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
                    pytest
                '''
            }
        }

        stage('Modify File and Push Back') {
            steps {
                script {
                 
                   withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                        sh '''
                            git config --global user.email "alikhaled09@gmail.com"
                            git config --global user.name "Jenkins"
                            git add build_log.txt
                            git commit -m "Auto update from Jenkins"
                            git push https://${TOKEN}@github.com/alikhaled09/simple_calc.git main
                        '''
                    }

                }
            }
        }
    }
}
