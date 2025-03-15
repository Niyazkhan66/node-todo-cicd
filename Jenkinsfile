pipeline {
    agent { label "dev-server" }

    stages {
        stage('Code Clone') {
            steps {
                git url: "https://github.com/Niyazkhan66/node-todo-cicd.git", branch: "master"
            }
        }
        stage('Code Build and Test') {
            steps {
                sh "docker build -t node-app ."
            }
        }
        stage('Push to Dockerhub') {
            steps {
                withCredentials([usernamePassword(credentialsId:"dockerHubCreds", passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]) {
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker tag node-app ${env.dockerHubUser}/node-app:latest"
                    sh "docker push ${env.dockerHubUser}/node-app:latest"
                } // Closing the withCredentials block here
            }
        }
        stage('Deploy') {
            steps {
                sh "docker compose down && docker compose up -d --build"
            }
        }
    }
}
