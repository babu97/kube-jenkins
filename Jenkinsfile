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

    stages{
        stage("Check out SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: ""
            }
        }

    }
}
}