pipeline {
    agent any

    tools {
        jdk 'jdk17'
        nodejs 'node16'
    }

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }

        stage('Checkout from Git') {
            steps {
                git branch: 'main', url: 'https://github.com/FokkerYe/deployment-of-youtube.git'
            }
        }

        stage('Install Dependencies') {
            steps {
                sh 'npm install'
            }
        }

        stage('Docker Build & Push') {
            steps {
                script {
                    withDockerRegistry(credentialsId: 'docker') {
                        sh 'docker build -t youtube-clone .'
                        sh 'docker tag youtube-clone monsyster/youtube-clone:latest'
                        sh 'docker push monsyster/youtube-clone:latest'
                    }
                }
            }
        }

        stage('Deploy to Container') {
            steps {
                script {
                    sh '''
                        docker rm -f youtube-clone || true
                        docker run -d --name youtube-clone -p 3000:3000 monsyster/youtube-clone:latest
                    '''
                }
            }
        }
    }
}
