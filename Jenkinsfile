pipeline {
    agent {
        label 'test'
    }
    tools{
        jdk 'jdk17'
        maven  'mvn3'
    }
    stages {
        stage ('clean Workspace'){
            steps{
                cleanWs()
            }
        }
        stage('Checkout') {
            steps {
            git branch: 'main', 
            credentialsId: 'git_cred', 
            url: 'https://github.com/Kabir9611/Boardgame.git'
            }
        }
        stage ('maven compile') {
            steps {
                sh 'mvn compile'
            }
        }
        stage ('maven Test') {
            steps {
                sh 'mvn test'
            }
        }
        stage ('maven Package') {
            steps {
                sh 'mvn package'
            }
        }
        stage ('Docker Image Build & Push') {
            steps {
             script{
                withDockerRegistry(credentialsId: '3a5fe225-62cd-476d-95b5-e8231466fe7f', toolName: 'docker') {
                    sh "docker build -t boardgame:latest ."
                    sh "docker tag boardgame:latest kabir96/boardgame:latest"
                    sh "docker push kabir96/boardgame:latest"
                 }
              }
            }
        }
        stage ('K8s deploy') {
            steps {
                sh 'minikube start'
                sh 'kubectl apply -f deployment-service.yaml'
            }
        }
    }
}
