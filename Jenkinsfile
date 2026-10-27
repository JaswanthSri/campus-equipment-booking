pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'feature/equipment-lookup-validation', 
                    url: 'https://github.com/JaswanthSri/campus-equipment-booking.git'
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
                // Transfer image directly to minikube via docker daemon
                sh 'docker save campus-equipment-booking:set-c | minikube image load -'
                
                // Apply manifests using elevated privileges pointing to host kubeconfig
                sh 'sudo kubectl --kubeconfig=/home/snucse/.kube/config apply -f deployment.yaml'
                sh 'sudo kubectl --kubeconfig=/home/snucse/.kube/config rollout status deployment/campus-equipment-deployment'
            }
        }
    }

    post {
        always {
            junit 'target/surefire-reports/*.xml'
        }
    }
}
