pipeline {
    agent any

    tools {
        jdk 'JDK-17'
        maven 'Maven-3'
    }

    environment {
        IMAGE_NAME = "petclinic"
        MAVEN_OPTS = "-Xmx1024m -XX:MaxMetaspaceSize=256m"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/Rosh123456789/spring-petclinic.git'
            }
        }

        stage('Build') {
            steps {
                // Skip tests to avoid JVM OOM on Windows
                bat 'mvn clean package -DskipTests'
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
                docker stop %IMAGE_NAME% >nul 2>&1
                docker rm %IMAGE_NAME% >nul 2>&1
                docker run -d --name %IMAGE_NAME% -p 8081:8080 %IMAGE_NAME%:%BUILD_NUMBER%
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
