pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', url: 'https://github.com/alikhaled09/simple_calc.git'
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                python3 -m venv venv
                . venv/bin/activate
                pip install --upgrade pip
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
                echo "Last build successful on $(date)" >> build_log.txt
                git config --global user.email "you@example.com"
                git config --global user.name "Ali Kamal"
                git add build_log.txt
                git commit -m "Auto update from Jenkins" || echo "No changes to commit"
                git push https://alikhaled09:${GITHUB_TOKEN}@github.com/alikhaled09/simple_calc.git main
                '''
            }
        }
    }
}
