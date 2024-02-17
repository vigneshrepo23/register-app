pipeline {
    agent {
        label "slave"
    }
    tools {
        jdk "java17"
        maven "maven3"
    }
    environment {
        APP_NAME = "registerapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "vigneshrepo23"
        DOCKER_PASS = "docker-token"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}" + "${BUILD_NUMBER}"
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

        stage ("build image") {
            steps {
                script {
                        docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                        }
                }
            }
        }

        stage ("push") {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                        docker_image.push('latest')
                    }
                }
            }
        }
    }
}
