pipeline {

    agent any
    environment {
        DOCKER_CONTEXT = 'default'
    }
    stages {
        stage('Build Jar') {
            agent {
                docker {
                    image 'maven:3.9.9-eclipse-temurin-21-jammy'
                    args '-u root -v /tmp/m2:/root/.m2'
                    reuseNode true
                }
            }
            steps{
                sh "mvn clean package -DskipTests"
            }
        }

        stage('Build Image') {
            steps{
                script {
                    app = docker.build("jiangao1/selenium:${env.BUILD_NUMBER}")
                }
            }
        }

        stage('Push Image') {
            steps{
                script {
                    docker.withRegistry('', 'dockerhub-creds') {
                        app.push("${env.BUILD_NUMBER}")
                    }
                }
            }
        }
    }

}