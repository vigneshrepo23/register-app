pipeline {
    agent {
        label "slave"
    }
    tools {
        jdk "java17"
        maven "maven3"
    }
    stages {
        stage ("clean workspace") {
            steps {
                cleanWs()
            }
        }
        
        stage ("git checkout") {
            steps {
                git branch: "main", credentialsId: "gitcred", url: "https://github.com/vigneshrepo23/register-app"
            }
        }

        stage ("build") {
            steps {
                sh "mvn clean package"
            }
        }

        stage ("test") {
            steps {
                sh "mvn test"
            }
        }

        stage ("sonar analysis") {
            steps {
                withSonarQubeEnv('sonarserver') { // system - sonarqube servers - name 
                sh 'mvn clean package sonar:sonar'
               }
            }
        }

        stage ("quality gates") {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
               }
            }
        }
    }
}
