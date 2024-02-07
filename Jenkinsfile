pipeline {
    agent { label "agent-server" }
    stages {
        stage("code clone") {
            steps {
                 git url: "https://github.com/Ravindra-799/dfsms.git", branch: "main"
            }
        }
        stage("Build and test") {
            steps {
                sh "docker build . -t dfsms-jenkins"
            }
        }
        stage("Push to DockerHub") {
           steps {
                withCredentials([usernamePassword(credentialsId:"docker-hub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                    sh "docker tag dfsms-jenkins ${env.dockerHubUser}/dfsms-jenkins:latest"
                    sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                    sh "docker push ${env.dockerHubUser}/dfsms-jenkins:latest"
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
