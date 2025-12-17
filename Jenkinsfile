pipeline {
    agent any

    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout From GitHub') {
            steps {
               git branch: 'main', url: 'https://github.com/arivaza97/Arivaz-Portfolio.git'
            }
        }
        stage ("Trivy File Scan") {
            steps {
               sh "trivy fs . > trivy.txt"
            }
        }
        stage('Build & Tag Docker Image') {
            steps {
                 script {
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker build -t arivuboi27/arivazportfolio:v1 ."
                    }
               }            
            }
        }
        stage('Docker Image Scan') {
            steps {
                sh "trivy image --format table -o trivy-image-report.html arivuboi27/arivazportfolio:v1 "
            }
        }
        stage('Push Docker Image') {
            steps {
               script {
                   withDockerRegistry(credentialsId: 'docker-cred', toolName: 'docker') {
                        sh "docker push arivuboi27/arivazportfolio:v1"
                    }
               }
            }
        }
        stage('Deploy on Kubernetes') {
            steps {
                sh 'sudo kubectl apply -f pod.yaml'
                sh 'sudo kubectl rollout restart deployment loadbalancer-pod'
            }
        }
        
    }
}
