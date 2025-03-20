pipeline {
    agent any
    environment {
        TOMCAT_PORT = credentials('tomcat-port')
        DOCKER_IMAGE = credentials('docker-image')
        DOCKER_CONTAINER_NAME = credentials('docker-container-name')
    }
    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'master',
                    credentialsId: 'github-credentials',
                    url: 'https://github.com/tletle7102/cicd.git'
            }
        }
        stage('Build Gradle') {
            steps {
                sh 'chmod +x ./gradlew'
                sh './gradlew clean build'
            }
        }
        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${DOCKER_IMAGE} ."
            }
        }
        stage('Deploy Docker Container') {
            steps {
                sh "docker stop ${DOCKER_CONTAINER_NAME} || true"
                sh "docker rm ${DOCKER_CONTAINER_NAME} || true"
                sh "docker run -d --name ${DOCKER_CONTAINER_NAME} -p ${TOMCAT_PORT}:8080 ${DOCKER_IMAGE}"
            }
        }
    }
}