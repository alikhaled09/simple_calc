pipeline {
    agent any

    environment {
        GITHUB_REPO = 'git@github.com:alikhaled09/simple_calc.git'
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
                // على Windows استخدم PowerShell
                powershell '''
                    python -m venv venv
                    .\\venv\\Scripts\\Activate.ps1
                    pip install --upgrade pip
                    pip install pytest
                '''
            }
        }

        stage('Run Tests') {
            steps {
                powershell '''
                    .\\venv\\Scripts\\Activate.ps1
                    pytest
                '''
            }
        }

        stage('Modify File and Push Back') {
            steps {
                script {
                    // أعمل log بالتاريخ
                    powershell 'date > build_log.txt'

                    // Add, commit, push عبر SSH
                    powershell '''
                        git config --global user.email "alikhaled09@gmail.com"
                        git config --global user.name "Jenkins"
                        git add build_log.txt
                        git commit -m "Auto update from Jenkins" -a || echo "No changes to commit"
                        git push origin main
                    '''
                }
            }
        }
    }
}
