pipeline{
    agent any
    tools{
        jdk 'jdk17'
        nodejs 'node16'
    }
    stages {
        stage('clean workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout from Git'){
            steps{
                git branch: 'main', url: 'https://github.com/FokkerYe/deployment-of-youtube.git'
            }
        }
        stage('Install Dependencies') {
            steps {
                sh "npm install"
            }
        }
        stage("Docker Build & Push"){
            steps{
                script{
                   withDockerRegistry(credentialsId: 'docker', toolName: 'docker'){   
                       sh "docker build -t youtube-clone ."
                       sh "docker tag youtube-clone monsyster/youtube-clone:latest "
                       sh "docker push monsyster/youtube-clone:latest "
                       sh "docker "
                    }
                }
            }
        }
        stage('Deploy to container'){
            steps{
                sh 'docker run -d --name youtube-clone -p 3000:3000 monsyster/youtube-clone:latest'
            }
        }
    }
}
