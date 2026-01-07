pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/<your-username>/spring-petclinic.git'
            }
        }

        stage('Build, Test & Checkstyle') {
            steps {
                bat 'mvn clean verify'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }
    }

    post {
        success {
            echo ' CI Pipeline Successful'
        }
        failure {
            echo ' CI Pipeline Failed'
        }
    }
}
