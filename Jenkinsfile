pipeline {
    agent any

    tools {
        jdk 'java17'
        maven 'maven3'
    }
    environment {
        APP_NAME = "complete-production-e2e-pipeline"
        RELEASE = "1.0.0"
        DOCKER_USER = "kipkulei"
        DOCKER_PASS = "dockerhub"
        IMAGE_NAME = "${DOCKER_USER}/${APP_NAME}"
        IMAGE_TAG = "${RELEASE}-${BUILD_NUMBER}"
    }
    stages {
        stage("Clean Workspace") {
        steps {
        dir("${WORKSPACE}") {
            deleteDir()
        }
        }
        }

        stage("Check out SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: "https://github.com/babu97/kube-jenkins.git"
            }
        }
        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }
        stage("Build & Push Docker Image") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build"${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS) {
                      docker_image.push("${IMAGE_TAG}")
                      docker_image.push("latest")
                    }
                }
            }
        }
    }
}
