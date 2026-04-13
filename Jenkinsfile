pipeline {
    agent any

    tools {
        maven '3.9.11'
    }

    environment {
        NEXUS_URL = 'http://host.docker.internal:8081/repository/maven_releases/'
        NEXUS_CREDENTIALS = 'jose1234'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Joselm11/my-gs-spring-boot.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean package -DskipTests'
            }
        }

        stage('Deploy to Nexus') {
            steps {
                script {
                    def jarFile = sh(
                        script: "ls target/*.jar | head -n 1",
                        returnStdout: true
                    ).trim()

                    def fileName = jarFile.tokenize('/').last()

                    sh """
                        curl -v -u admin:jose1234 \
                        --upload-file target/*.jar \
                        http://host.docker.internal:8081/repository/maven_releases/
                    """
                }
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }
}
