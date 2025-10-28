pipeline {
    agent any

    environment {
        GITHUB_REPO = 'https://github.com/alikhaled09/simple_calc.git'
        BRANCH_NAME = 'main'
    }

    stages {
        stage('Clone Repo') {
            steps {
                withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                    git branch: "${BRANCH_NAME}", url: "https://${TOKEN}@github.com/alikhaled09/simple_calc.git"
                }
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
                    sh 'date > build_log.txt'
                    withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                        sh '''
                            git config --global user.email "alikhaled09@gmail.com"
                            git config --global user.name "Jenkins"
                            git add build_log.txt
                            git commit -m "Auto update from Jenkins" || echo "Nothing to commit"
                            git push https://${TOKEN}@github.com/alikhaled09/simple_calc.git main
                        '''
                    }
                }
            }
        }
    }
}
