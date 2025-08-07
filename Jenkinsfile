pipeline{
    agent any

    environment {
        VENV = 'venv'
    }

    triggers {
        pollSCM('H/5 * * * *')
    }

    stages {
        stage('Cloning Repo from github') {
            steps {
                echo 'Cloning the github repo'
                git branch: 'main', url: 'https://github.com/KaranManhas22/MediBot.git'
            }
        }
        stage('Setup Python virtual Environment') {
            steps {
                echo 'Creating virtual environment'
                sh 'python3 -m venv $VENV'
                sh '. $VENV/bin/activate'
            }
        }
        stage('Install python dependencies') {
            steps {
                sh '. $VENV/bin/activate && pip freeze > requirements.txt'
                sh '. $VENV/bin/activate && pip install -r requirements.txt'
                sh '. $VENV/bin/activate && pip install numpy'
                sh '. $VENV/bin/activate && pip install pandas'
                sh '. $VENV/bin/activate && pip install flask'
                sh '. $VENV/bin/activate && pip install scikit-learn==1.6.1'
                sh '. $VENV/bin/activate && pip install gunicorn'
            }
        }
        stage('run') {
            steps {
                sh './$VENV/bin/gunicorn --bind 0.0.0.0:8000 main:app'
            }
        }
    }
}