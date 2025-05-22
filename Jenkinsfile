@Library("Shared") _
pipeline {
    agent {label "deep"}

    stages {
        stage('Hello') {
            steps {
                script {
                    hello()
                }
            }
        }
        stage('Code') {
            steps {
                echo 'This is cloning the code'
                git url:"https://github.com/DipanshuKumrawal/django-notes-app", branch:"dev"
                echo 'Code Cloning Successful'
            }
        }
        stage('Build') {
            steps {
                echo 'This is building the code'
                sh "docker build -t notes-app:latest ."
            }
        }
        stage('Push to Docker Hub') {
            steps {
                echo 'This is pushing the image to docker hub'
                withCredentials([usernamePassword(credentialsId: 'DockerHubCred', usernameVariable: 'DockerHubUser', passwordVariable: 'DockerHubPass')]){
                    sh "docker login -u ${env.DockerHubUser} -p ${env.DockerHubPass}"
                    sh "docker image tag notes-app:latest ${env.DockerHubUser}/notes-app:latest"
                    sh "docker push ${env.DockerHubUser}/notes-app:latest"
                }
            }
        }
        stage('Deploy') {
            steps {
                echo 'This is deploying the code'
                sh "docker compose down && docker compose up -d"
            }
        }
    }
}
