pipeline {
    agent any

    environment {
        APP_PORT = '80' // Host port to expose the app on
        CONTAINER_PORT = '5000' // Flask app default port
    }

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
                    echo "Freeing up port $APP_PORT if already in use..."
                    if lsof -i :$APP_PORT; then
                        echo "Port $APP_PORT is in use. Killing process..."
                        fuser -k ${APP_PORT}/tcp || true
                    else
                        echo "Port $APP_PORT is free."
                    fi

                    echo "Removing existing container if exists..."
                    docker rm -f sample-app-container || true

                    echo "Starting new container: host port $APP_PORT -> container port $CONTAINER_PORT..."
                    docker run -d --name sample-app-container -p $APP_PORT:$CONTAINER_PORT sample-app
                '''
            }
        }

        stage('Deploy with Ansible') {
            steps {
                sh 'ansible-playbook deploy.yml'
            }
        }
    }
}
