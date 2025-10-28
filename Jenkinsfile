pipeline {
    agent any

    environment {
        GIT_REPO = 'https://github.com/alikhaled09/simple_calc.git'
        BRANCH = 'main'
    }

    stages {
        stage('Clone Repo') {
            steps {
                withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                    sh """
                        git config --global user.email "alikhaled09@gmail.com"
                        git config --global user.name "Jenkins"
                        git clone -b ${BRANCH} https://$TOKEN@github.com/alikhaled09/simple_calc.git repo
                    """
                }
            }
        }

        stage('Install Requirements') {
            steps {
                sh '''
                    cd repo
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
                    cd repo
                    . venv/bin/activate
                    pytest
                '''
            }
        }

        stage('Modify and Push Back') {
            steps {
                withCredentials([string(credentialsId: 'github-token', variable: 'TOKEN')]) {
                    sh '''
                        cd repo
                        date > build_log.txt
                        git add build_log.txt
                        git commit -m "Auto update from Jenkins"
                        git push https://$TOKEN@github.com/alikhaled09/simple_calc.git main
                    '''
                }
            }
        }
    }
}
