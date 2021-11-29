pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'apk add python3'
                sh 'apk add py3-pip'
                sh 'apk add --no-cachce --virtual .build-deps gcc libc-dev linux-headers python3-dev postgresql-dev mariadb-dev g++ mysql'
                sh 'pip install -r requirements.txt'
                sh 'pip install --upgrade pip'
                sh 'pip install flask-wtf'
                sh 'pip install bandit'
                sh 'pip install safety'
            }
        }
        stage('Test') {
                    steps {
                        sh 'python3 testAuth.py'
                    }
                }
        stage('OWASP Dependency check') {
                    steps {
                        dependencyCheck additionalArguments: '--format HTML --format XML', odcInstallation: 'Default'
                    }
        }
        stage ("Python Bandit Security Scan"){
              steps{
                sh "bandit -r app.py "
                sh "bandit -r templates"
                sh "bandit -r static"
                sh "bandit -r App"
              }
        }
        stage ("Dependency Check with Python Safety"){
                      steps{
                        sh "safety check"
                      }
                }
    }
    post {
    		success {
    			dependencyCheckPublisher pattern: 'dependency-check-report.xml'
    		}
    }
}