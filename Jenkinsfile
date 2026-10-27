pipeline {
    agent any

    tools {
        maven 'Maven 3' // Or whichever Maven name is configured in your Jenkins Global Tool Configuration
        jdk 'Java 17'
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/equipment-lookup-validation', 
                    url: 'https://github.com/YOUR_USERNAME/campus-equipment-booking.git'
            }
        }

        stage('Build & Test') {
            steps {
                sh 'mvn clean test'
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t campus-equipment-booking:set-c .'
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'minikube image load campus-equipment-booking:set-c'
                sh 'kubectl apply -f deployment.yaml'
                sh 'kubectl rollout status deployment/campus-equipment-deployment'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
