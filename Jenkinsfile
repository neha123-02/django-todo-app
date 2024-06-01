pipeline {
    agent any
    stages {
        stage("Code Cloned") {
            steps {
                git url: "https://github.com/neha123-02/django-todo-app.git", branch: "main"
                echo "Code Cloned"
            }
        }
        stage("Code Build") {
            steps {
                sh "docker build . -t django-app"
                echo "Code Built"
            }
        }
        stage("Pushing the image to the docker hub") {
            steps {
                withCredentials([usernamePassword(credentialsId: "dockerhub", passwordVariable: "dockerhubpass", usernameVariable: "dockerhubuser")]) {
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpass}"
                    sh "docker tag django-app:latest ${env.dockerhubuser}/django-app:latest"
                    sh "docker push ${env.dockerhubuser}/django-app:latest"
                }
                echo "Pushed the image to Docker Hub"
            }
        }
        stage("Deploying the application") {
            steps {
                sh "docker compose down && docker compose up -d"
                echo "Application got deployed"
            }
        }
    }
}
