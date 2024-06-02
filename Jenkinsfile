pipeline {
    agent any

    environment {
        DOCKER_HUB_CREDENTIALS = credentials('dockerhub-credentials') // Assuming a single credential for both username and password

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

        stage('Build Docker Image') {
            steps {
                script {
                    def version = "1.0.${env.BUILD_NUMBER}" // Example versioning
                    def dockerImageName = "${DOCKER_HUB_CREDENTIALS_USR}/${DOCKER_HUB_REPOSITORY_NAME}:${version}"
                    sh "docker build -t ${dockerImageName} ."."
                }
            }
        }

stage('Push Docker Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_HUB_PASSWORD', usernameVariable: 'DOCKER_HUB_USERNAME')]) {
                    sh 'docker login -u "${DOCKER_HUB_USERNAME}" -p "${DOCKER_HUB_PASSWORD}"'
                    sh "docker push ${DOCKER_HUB_CREDENTIALS_USR}/${DOCKER_HUB_REPOSITORY_NAME}:${version}"
                }
            }
        }
    }
}
