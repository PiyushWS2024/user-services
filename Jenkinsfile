pipeline {
    agent any

    tools {
        maven "MAVEN"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/PiyushWS2024/user-services.git'
            }
        }

        stage('Compile') {
            steps {
                sh "mvn clean compile"
            }
        }

        stage('Build') {
            steps {
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t PiyushWS2024/user-services:latest .'
            }
        }

        stage('Docker Login') {
            steps {
                withCredentials([string(credentialsId: 'DOCKER_HUB_PASSWORD', variable: 'PASSWORD')]) {
                    sh 'docker login -u javaexpress -p $PASSWORD'
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push PiyushWS2024/user-services:latest'
            }
        }

        stage("Kubernetes Deployment") {
            steps {
                sh '''
                    kubectl delete deployment user-service-deployment --ignore-not-found
                    sleep 5
                    kubectl apply -f user_deployment.yaml
                    kubectl rollout status deployment user-service-deployment
                '''
            }
        }
    }
}
