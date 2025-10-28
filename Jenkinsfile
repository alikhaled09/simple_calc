pipeline {
    agent {
        docker {
            image 'python:3.10'
        }
    }

    stages {
        stage('Clone Repo') {
            steps {
                git 'https://github.com/alikhaled09/simple_calc.git'
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                    python3 -m venv venv
                    . venv/bin/activate
                    pip install -r requirements.txt
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
                sh '''
                    echo "Build done at $(date)" >> build_log.txt
                    git config user.email "you@example.com"
                    git config user.name "Jenkins"
                    git add build_log.txt
                    git commit -m "Auto update from Jenkins"
                    git push https://github.com/alikhaled09/simple_calc.git main || echo "No changes to push"
                '''
            }
        }
    }
}
