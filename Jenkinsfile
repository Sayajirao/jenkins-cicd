pipeline {
    agent any 
    
    stages {
        stage("Code") {
            steps {
                echo "Cloning the code" 
                git url: "https://github.com/kkeshav007/jenkins-cicd.git", branch: "main"
            }
        }
        stage("Build") {
            steps {
                echo "Building the code"
                sh "docker build -t my-note-app ."
            }            
        }
        stage("Push to Docker Hub") {
            steps {
                echo "Pushing the image to Docker Hub"
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'DOCKERHUB_USERNAME', passwordVariable: 'DOCKERHUB_PASSWORD')]) {
                    sh 'docker login -u $DOCKERHUB_USERNAME -p $DOCKERHUB_PASSWORD'
                    sh 'docker tag my-note-app $DOCKERHUB_USERNAME/my-note-app:latest'
                    sh 'docker push $DOCKERHUB_USERNAME/my-note-app:latest'
                }
            }
        }
        stage("Deploy") {
            steps {
                echo "Deploying the container"
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
