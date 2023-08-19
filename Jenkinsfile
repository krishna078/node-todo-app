pipeline {
    agent { label 'Dev-agent' } 
    
    stages {
        stage('cloned') {
            steps {
                git url: "https://github.com/krishna078/node-todo-app.git", branch: "master"
            }
        }
        stage('Build and Test'){
            steps {
               sh 'docker build . -t node-todo-app:latest'
            }
        }
        stage('login and push image'){
            steps {
               echo 'Logging in to Docker Hub'
               withCredentials([usernamePassword(credentialsId: 'dockerhub', usernameVariable: 'dockerhubuser', passwordVariable: 'dockerhubpassword')]) {
                    sh "docker login -u ${env.dockerhubuser} -p ${env.dockerhubpassword} || echo 'Docker login failed'"
                    sh "docker push ${env.dockerhubUser}/node-todo-app:latest"
                    
               }
            }
        }
        stage('Deploy'){
            steps {
                sh 'docker-compose down && docker-compose up -d'
            }
        }
    }
}
