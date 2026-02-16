pipeline {
    agent any

    tools {
        nodejs "Node18"
    }

    stages {

        stage('Clone') {
            steps {
                git url: 'https://github.com/athulkaratte/calcium.git', branch: 'main'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --legacy-peer-deps'
            }
        }

        stage('Build') {
            steps {
                sh 'NODE_OPTIONS=--max_old_space_size=4096 npm run build'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t calcium-app .'
            }
        }

        stage('Deploy') {
            steps {
                sh """
                docker stop calcium || true
                docker rm calcium || true
                docker run -d -p 3000:3000 --name calcium calcium-app
                """
            }
        }
    }
}
