pipeline{
    agent{
        label "jenkins"
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    stages{
        stage("Clean Workspace"){
            steps{
                cleanWs()
            }
        }

        stage("Check out SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: "https://github.com/babu97/kube-jenkins.git"
            }
        }
        stage("Build Application"){
            steps{
                sh "mvn clean package"
            }
        }

        stage("Test Application"){
            steps{
                sh "mvn test"
            }
        }

    }
}
