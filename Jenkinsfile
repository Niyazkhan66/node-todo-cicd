pipeline {
    agent { label 'dev-agent'}
    
    stages {
        stage('Code'){
            steps {
                git url: 'https://github.com/Niyazkhan66/node-todo-cicd.git', branch: 'master'
            }
        }
           stage('Build and Test'){
            steps {
                sh 'docker build . -t niyazkhan66/node-todo-app-cicd:latest'
            }
        }
        stage('Login and Push'){
            steps {
                echo "loging in to docker hub and pushing image"
                withCredentials([usernamePassword(credentialsId: 'dockerHub', passwordVariable: 'dockerHubPassword', usernameVariable: 'dockerHubUser')]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPassword}"
                    sh "docker push niyazkhan66/node-todo-app-cicd:latest"
                }
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d --no-deps --build web' 
            }
        }
    }
}
