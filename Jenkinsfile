pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3'
    }

    environment {
        IMAGE_NAME = "petclinic"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Rosh123456789/spring-petclinic.git'
            }
        }

        stage('Build & Verify') {
            steps {
                bat 'mvn clean verify'
            }
        }

        stage('Package') {
            steps {
                bat 'mvn package -DskipTests'
            }
        }

        stage('Docker Build') {
            steps {
                bat 'docker build -t %IMAGE_NAME%:%BUILD_NUMBER% .'
            }
        }

        stage('Docker Deploy') {
            steps {
                bat '''
                docker stop %IMAGE_NAME% || exit 0
                docker rm %IMAGE_NAME% || exit 0
                docker run -d --name %IMAGE_NAME% -p 8080:8080 %IMAGE_NAME%:%BUILD_NUMBER%
                '''
            }
        }
    }

    post {
        success {
            echo ' CI/CD with Docker completed successfully'
        }
        failure {
            echo ' CI/CD pipeline failed'
        }
    }
}
