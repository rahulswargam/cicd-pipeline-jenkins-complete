pipeline {
    agent any

    environment {
        DOCKERHUB_CREDENTIALS = credentials('dockerhub-credentials')
        KUBECONFIG_CREDENTIALS = credentials('kubeconfig-credentials')
        DOCKERHUB_REPO = 'swargamrahul02/devopsassignment'
    }

    stages {
        stage('Clone Repository') {
            steps {
                git 'https://github.com/rahulswargam/cicd-pipeline-jenkins-complete.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    dockerImage = docker.build("${env.DOCKERHUB_REPO}:${env.BUILD_ID}")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                script {
                    docker.withRegistry('', 'dockerhub-credentials') {
                        dockerImage.push()
                        dockerImage.push('latest')
                    }
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                withKubeConfig([credentialsId: 'kubeconfig-credentials']) {
                    sh 'kubectl apply -f k8s-deployment.yaml'
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}

