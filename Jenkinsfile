pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main',
                url: 'https://github.com/YOUR_GITHUB_USERNAME/SampleMERNwithMicroservices.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh '''
                npm install
                '''
            }
        }

        stage('Build') {
            steps {
                sh '''
                echo "Building MERN App..."
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                pm2 stop all || true
                pm2 start npm -- start
                pm2 save
                '''
            }
        }
    }
}
