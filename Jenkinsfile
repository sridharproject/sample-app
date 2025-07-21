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
                    echo "Stopping any container using port 8080..."
                    docker ps --filter "publish=8080" --format "{{.ID}}" | xargs -r docker stop

                    echo "Removing old container if exists..."
                    docker rm -f sample-app-container || true

                    echo "Starting new container on port 8080..."
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
