pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps { checkout scm }
        }

        stage('Build') {
            steps { sh 'docker compose build' }
        }

        stage('Run') {
            steps { sh 'docker compose up -d' }
        }

        stage('Test') {
            steps {
                sh '''
                    for i in $(seq 1 15); do
                        curl -sf http://localhost:8000 && exit 0
                        sleep 2
                    done
                    echo "App did not respond"; exit 1
                '''
            }
        }
    }

    post {
        always {
            sh 'docker compose logs || true'
            sh 'docker compose down -v || true'
        }
    }
}
