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
                    docker rm -f sample-app-container || true
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
