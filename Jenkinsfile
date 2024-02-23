pipeline {
    agent {
        label 'slave'
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    stages {
        stage ('cleanws') {
            steps {
                cleanWs()
            }
        }
        stage ('checkout') {
            steps {
                git branch: 'main', credentialsId: 'gitcred', url: 'https://github.com/vigneshrepo23/register-app'
            }
        }
        stage ('build') {
            steps {
                sh "mvn clean package"
            }
        }
        stage ('test') {
            steps {
                sh "mvn test"
            }
        }
    } 
}
