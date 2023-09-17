pipeline {
    agent { label "dev-server" }
    stages{
        stage("Clone Code"){
            steps{
                git url: "https://github.com/KvSanojKV/node-todo-cicd.git", branch: "master"
            }
        }
        stage("Build and Test"){
            steps{
             sh "docker build -t node-app-test ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubpass",usernameVariable:"dockerhubuser")]){
                sh "docker tag node-app-test ${env.dockerhubuser}/node-app-test:latest"
                sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                sh "docker push ${env.dockerhubuser}/node-app-test:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
            echo "Docker will deploy the image"
            sh "docker compose down && docker compose up -d"
            }
        }
    }
}
