pipeline {
    agent {
        label 'slave'
    }
    tools {
        jdk 'java17'
        maven 'maven3'
    }
    environment {
        APP_NAME = "registerapp"
        RELEASE = "1.0.0"
        DOCKER_USER = "vigneshrepo23"
        DOCKER_PASS = "dockercred"
        IMAGE_NAME = "${DOCKER_USER}" + "/" + "${APP_NAME}"
        IMAGE_TAG = "${RELEASE}" + "${BUILD_NUMBER}"
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
        stage ('analysis') {
            steps {
                withSonarQubeEnv('sonarserver') {
                sh 'mvn clean package sonar:sonar'
               }
            }
        }
        stage('quality gates') {
            steps {
                timeout(time: 1, unit: 'MINUTES') {
                waitForQualityGate abortPipeline: true
               }
            }
        }
        stage('docker build & push') {
            steps {
                script {
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image = docker.build "${IMAGE_NAME}"
                    }
                    docker.withRegistry('',DOCKER_PASS) {
                        docker_image.push("${IMAGE_TAG}")
                    }
                }
            }
        } 
        stage ('trivy') {
            steps {
                sh "trivy image ${IMAGE_NAME} > /home/ubuntu/trivyreport.txt"
            }
        }
        stage ('artifacts cleanup') {
            steps {
                sh 'docker rmi ${DOCKER_USER}/${APP_NAME}'
            }
        }
    } 
}
