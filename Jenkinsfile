pipeline {
    agent {
     label 'my-cloud-label'

    }
    }
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
        
        stage("SonarQube Analysis") {
            steps {
                script {
                    // Start SonarQube environment
                    withSonarQubeEnv(credentialsId: 'jenkins-sonar-token') {
                        // Perform actions within the SonarQube environment
                        // For example, execute SonarQube scanner
                        sh 'mvn sonar:sonar'
                    }
                }
            }
        }
        stage("Quality Gate") {
            steps {
                script {
                    // Start SonarQube environment
                     waitForQualityGate abortPipeline: false, credentialsId: 'jenkins-sonar-token'
                        // Perform actions within the SonarQube environment
                        // For example, execute SonarQube scanne
                    }
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
