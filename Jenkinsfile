pipeline {
    agent any
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/itztejal/node-app.git", branch: "main"
                echo 'code is cloned'
            }
        }
        stage("build and test"){
            steps{
                sh "docker build -t node-app-test-new ."
                echo 'code is build and tested'
            }
        }
        stage("scan image"){
            steps{
                echo 'image scanning'
            }
        }
        stage("push"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app:latest ${env.dockerHubUser}/node-app:latest"
                sh "docker push ${env.dockerHubUser}/node-app:latest"
                echo 'image push'
                }
            }
        }
        stage("deploy"){
            steps{
                sh "docker-compose down && docker-compose up -d"
                echo 'done the deployment '
            }
        }
    }
}
