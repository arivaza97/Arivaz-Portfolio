pipeline {
    agent any

    stages {
        stage('Pull Code From GitHub') {
            steps {
                git 'https://github.com/arivaza97/Arivaz-Portfolio.git'
            }
        }
        stage('Build the Docker image') {
            steps {
                sh 'sudo docker build -t ArivazPortfolio /var/lib/jenkins/workspace/weekendproj'
                sh 'sudo docker tag ArivazPortfolio arivaza97/ArivazPortfolio:latest'
                sh 'sudo docker tag ArivazPortfolio arivaza97/ArivazPortfolio:${BUILD_NUMBER}'
            }
        }
        stage('Push the Docker image') {
            steps {
                sh 'sudo docker image push arivaza97/ArivazPortfolio:latest'
                sh 'sudo docker image push arivaza97/ArivazPortfolio:${BUILD_NUMBER}'
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f /var/lib/jenkins/workspace/weekendproj/pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
    }
}
