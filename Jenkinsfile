pipeline {
    agent any
    stages {
        stage('Clone Repo') {
            steps {
                git credentialsId: 'github-creds', url: 'https://github.com/sridharproject/sample-app.git', branch: 'main'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t sample-app .'
            }
        }

        stage('Push to DockerHub') {
            steps {
                sh '''
                    echo "Sree$#2805SS" | docker login -u "Sridharproject" --password-stdin
                    docker tag sample-app sridharproject/sample-app
                    docker push sridharproject/sample-app
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
