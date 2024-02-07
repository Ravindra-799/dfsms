pipeline {
    agent { label "agent-server" }
    stages {
        stage("code clone") {
            steps {
                 git url: "https://github.com/SUBBARAMIREDDY-K/ttms-conterized.git", branch: "main"
            }
        }
        stage("Build and test") {
            steps {
                sh "docker build . -t ttms-jenkins"
            }
        }
        stage("Push to DockerHub") {
           steps {
                withCredentials([usernamePassword(credentialsId:"docker-hub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag ttms-jenkins ${env.dockerHubUser}/ttms-jenkins:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/ttms-jenkins:latest"
                }
           }
        }
        stage("Deploy") {
            steps {
                sh "docker-compose down && docker-compose up -d"
            }
            
        }
    }
}