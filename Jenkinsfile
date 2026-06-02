pipeline {
    agent any 

    tools {
        maven 'maven-3' 
        jdk 'jdk-11'
    }

    stages {
        stage('Build App') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Test') {
            steps {
                sh 'mvn test'
            }
        }

        stage('Build Image') {
            steps {
                script {
                    sh 'docker build -t java-app .'
                }
            }
        }

        stage('Push DockerHub') {
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: '1122334455', passwordVariable: 'DOCKER_PASS', usernameVariable: 'DOCKER_USER')]) {
                        
                        sh 'echo "$DOCKER_PASS" | docker login -u "$DOCKER_USER" --password-stdin'
                        
                        sh 'docker tag java-app $DOCKER_USER/java-app:${BUILD_NUMBER}'
                        sh 'docker tag java-app $DOCKER_USER/java-app:latest'
                        
                        sh 'docker push $DOCKER_USER/java-app:${BUILD_NUMBER}'
                        sh 'docker push $DOCKER_USER/java-app:latest'
                    }
                }
            }
        }
    }
}
