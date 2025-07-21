pipeline {
    agent any

    stages {
        stage('Clone Repo') {
            steps {
                git credentialsId: 'GITHUB_CRED', url: 'https://github.com/sridharproject/sample-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-app .'
            }
        }

        stage('Run Docker Container Locally') {
            steps {
                sh '''
                    export APP_PORT=8081

                    echo "Freeing up port $APP_PORT if already in use..."
                    if lsof -i :$APP_PORT; then
                        echo "Port $APP_PORT is in use. Killing process..."
                        fuser -k ${APP_PORT}/tcp || true
                    else
                        echo "Port $APP_PORT is free."
                    fi

                    echo "Removing existing container if exists..."
                    docker rm -f sample-app-container || true

                    echo "Starting new container on port $APP_PORT..."
                    docker run -d --name sample-app-container -p $APP_PORT:8080 sample-app
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook ansible/deploy.yml'
            }
        }
    }
}
