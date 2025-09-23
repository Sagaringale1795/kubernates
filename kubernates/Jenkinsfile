pipeline {
    agent any
    environment {
        DOCKER_IMAGE = "sagaringale03/sagar2"
        COMMIT_ID = '' // initialize
    }
    stages {
        stage('Checkout Code') {
            steps {
                echo "Checking out repository..."
                git branch: 'master', url: 'https://github.com/Sagaringale1795/kubernates.git'
                echo "Checkout complete"
            }
        }

        stage('Set Commit ID') {
            steps {
                script {
                    env.COMMIT_ID = sh(script: "git rev-parse --short HEAD", returnStdout: true).trim()
                    echo "Commit ID: ${env.COMMIT_ID}"
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                echo "Building Docker image with tags: latest and ${env.COMMIT_ID}..."
                sh "docker build -t $DOCKER_IMAGE:latest -t $DOCKER_IMAGE:${env.COMMIT_ID} ./kubernates"
            }
        }

        stage('Push Docker Image') {
            steps {
                echo "Pushing image to Docker Hub..."
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', usernameVariable: 'USER', passwordVariable: 'PASS')]) {
                    sh "echo $PASS | docker login -u $USER --password-stdin"
                    sh "docker push $DOCKER_IMAGE:latest"
                    sh "docker push $DOCKER_IMAGE:${env.COMMIT_ID}"
                }
                echo "Push complete"
            }
        }
    }

    post {
        success {
            echo "Pipeline finished successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
