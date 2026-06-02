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
                    withCredentials([usernamePassword(credentialsId: '1122334455', passwordVariable: 'Bebo20010873Bebo', usernameVariable: 'abdelrahimbebo')]) {
                        sh 'docker login --username $Username --password $Password'
                        sh 'docker tag java-app $Username/java-app:${env.BUILD_NUMBER}'
                        sh 'docker tag java-app $Username/java-app:latest'
                        sh 'docker push $Username/java-app:${env.BUILD_NUMBER}'
                        sh 'docker push $Username/java-app:latest'
                    }
                }
            }
        }
    }
}
