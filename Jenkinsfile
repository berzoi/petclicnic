pipeline {
    agent any

    environment {
        DOCKER_HUB_REPOSITORY_NAME = 'petclinic'
    }

    stages {
        stage('Compile') {
            steps {
                sh 'mvn clean compile'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Project') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Build and Push Docker Image') {
            steps {
                script {
                    def buildNumber = env.BUILD_NUMBER
                    withCredentials([usernamePassword(credentialsId: 'docker-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    def dockerImageName = "${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY_NAME}:${buildNumber}"
                    sh "docker build -t ${dockerImageName} ."
                    sh 'docker login -u "${DOCKER_HUB_USERNAME}" -p "${DOCKER_HUB_PASSWORD}"'
                    sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY_NAME}:${env.BUILD_NUMBER}"
                }
            }
        }
    }
}
