pipeline {
    agent any
    stages {
        stage('Clone Code') {
            steps {
                // Build steps go here
                git url: 'https://github.com/estebanmorenoit/node-todo-cicd.git', branch: 'master'
            }
        }
        stage("Build"){
            steps{
                sh 'docker build . -t node-todo-cicd'
            }
        }
        stage("Push to Docker Hub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerhub",passwordVariable:"dockerhubPass",usernameVariable:"dockerhubUser")]){
                sh "docker tag node-todo-cicd ${env.dockerhubUser}/node-todo-cicd:latest"
                sh "docker login -u ${env.dockerhubUser} -p ${env.dockerhubPass}"
                sh "docker push ${env.dockerhubUser}/node-todo-cicd:latest"
                }
            }
        }
        stage("Deploy"){
            steps{
                sh 'docker compose down && docker compose up -d'
            }
        }
    }
}