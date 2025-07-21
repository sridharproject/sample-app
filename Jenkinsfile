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
                    echo "Freeing up port 8080 if already in use..."
                    if lsof -i :8080; then
                        echo "Port 8080 is in use. Killing process..."
                        fuser -k 8080/tcp || true
                    else
                        echo "Port 8080 is free."
                    fi

                    echo "Removing existing container if exists..."
                    docker rm -f sample-app-container || true

                    echo "Starting new container..."
                    docker run -d --name sample-app-container -p 8080:8080 sample-app
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
