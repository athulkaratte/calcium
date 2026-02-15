pipeline {
    agent any

    tools {
        nodejs "Node18"
    }

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/athulkaratte/calcium.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install --force'
'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'npm run test -- --watchAll=false'
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

        stage('Deploy Container') {
            steps {
                sh '''
                docker stop calcium || true
                docker rm calcium || true
                docker run -d -p 3000:3000 --name calcium calcium-app
                '''
            }
        }
    }
}
