pipeline {

// some comment2
    agent any

    environment{
        DOCKER_HUB_USERNAME = 'berzoi'
        DOCKER_HUB_REPOSITORY_NAME = 'petclinic'
    }
    
    tools{
        jdk 'jdk8'
        maven 'maven3'
    }

    stages{
        stage("Compile"){
            steps{
                sh "mvn clean compile"
            }
        }

         stage("Test Cases"){
            steps{
                sh "mvn test"
            }
        }

         stage("Build"){
            steps{
                sh "mvn clean install"
            }
        }
        stage("Build docker image"){
            steps{
                sh "docker build -t ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY_NAME}:$env.BUILD_NUMBER ."
            }
        }
        stage("Publish docker image"){
            steps{
                script{
                    withCredentials([usernamePassword(credentialsId: "docker-credentials", usernameVariable: "DOCKER_REPOSITORY_USER", passwordVariable: "DOCKER_REPOSITORY_PASSWORD")]){
                        sh "docker login -u $DOCKER_REPOSITORY_USER -p $DOCKER_REPOSITORY_PASSWORD"
                        sh "docker push ${DOCKER_HUB_USERNAME}/${DOCKER_HUB_REPOSITORY_NAME}:$env.BUILD_NUMBER"
                    }
                }
            }
        }
    }
}
