pipeline {
    agent any 
    stages {
        stage("Clone Code from Github") {
            steps {
                echo "Clonning from github"
                git url: "https://github.com/Harshtyagi001/django-notes-app-jenkins-declarative", branch:"main"
            }
        }
        stage("Building the Image"){
            steps{
                 echo "Building thr image"
                 sh "docker build -t my-notes-app ."
            }
        }
        stage("Push to Docker Hub"){
            steps{
                 echo "Push to Docker Hub"
                 withCredentials([usernamePassword(credentialsId:"dockerHubCredentials",passwordVariable:"dockerHubPass",usernameVariable:"dockerHubUser")]){
                          sh "docker tag my-notes-app ${env.dockerHubUser}/my-notes-app:latest"
                          sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                          sh "docker push ${env.dockerHubUser}/my-notes-app:latest"
                 }
            }
        }
        stage("Deploying the container"){
            steps{
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
            }
        }
    }
}
