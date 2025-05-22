pipeline {
    agent any

    stages {
        stage('Git Clone') {
            steps {
                git branch: 'master', url: 'https://github.com/Vishnupriyalnm/Insurance_Project.git'
                sh 'mvn clean package'
            }
        }
        stage('Build docker image') {
            steps {
                sh 'docker build -t vishnupriyalnm/insurancedemo:1 .'
            }
        }
        stage('Docker image to dockerhub') {
            steps {
                withCredentials([string(credentialsId: 'dockerhubpass', variable: 'dockerhubpass')]) {
                 sh "docker login -u  vishnupriyalnm -p ${dockerhubpass}"
                 sh "docker push vishnupriyalnm/insurancedemo:1"
}
            }
        }
       stage('Deploy Using Docker') {
            steps {
                script {
                    sh 'docker stop insurancedemo || true'
                    sh 'docker rm insurancedemo || true'
                    sh 'docker run -d --name insurancedemo -p 9090:8081 vishnupriyalnm/insurancedemo:1'
                }
            }
        }
    }
}
