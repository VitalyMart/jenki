pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }
        stage('Build Docker Image') {
            steps {
                script {
                    def image = docker.build("jenki_image:${env.BUILD_ID}")
                }
            }
        }
        stage('Deploy') {
            steps {
                script {
                    sh 'docker stop jenki_container || true'
                    sh 'docker rm jenki_container || true'
                    sh "docker run -d --name jenki_container -p 8081:8080 jenki_image:${env.BUILD_ID}"
                }
            }
        }
    }
    post {
        always {
            script {
                sh 'docker image prune -f'
            }
        }
    }
}
