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
                bat '''
                python -m venv venv
                call venv\\Scripts\\activate
                pip install --upgrade pip
                pip install -r requirements.txt
                '''
            }
        }

        stage('Run Tests') {
            steps {
                bat '''
                call venv\\Scripts\\activate
                pytest
                '''
            }
        }

        stage('Modify File and Push Back') {
            steps {
                bat '''
                echo "Last build successful on %date% %time%" >> build_log.txt
                git config --global user.email "you@example.com"
                git config --global user.name "Ali Kamal"
                git add build_log.txt
                git commit -m "Auto update from Jenkins"
                git push origin main
                '''
            }
        }
    }
}
