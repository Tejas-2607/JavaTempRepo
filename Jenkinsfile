pipeline {
    agent any

    environment {
        DOCKER_IMAGE = 'tejasjahagirdar/javatemprepo'
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Tejas-2607/JavaTempRepo.git'
            }
        }

        stage('Build Application') {
            steps {
                bat 'mvn clean package'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    docker.build("${DOCKER_IMAGE}:latest")
                }
            }
        }

        stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-creds', usernameVariable: 'tejasjahagirdar', passwordVariable: 'Tejasss@123')]) {
                    bat """
                    echo $PASS | docker login -u $USER --password-stdin
                    docker push ${DOCKER_IMAGE}:latest
                    """
                }
            }
        }

        stage('Run Docker Container') {
            steps {
                bat """
                docker stop javatemprepo || true
                docker rm javatemprepo || true
                docker run -d --name javatemprepo -p 8080:8080 ${DOCKER_IMAGE}:latest
                """
            }
        }
    }
}
