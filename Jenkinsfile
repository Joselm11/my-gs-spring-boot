pipeline {
    agent any

    tools {
        maven '3.9.11'
    }

    environment {
        NEXUS_URL = "http://host.docker.internal:8081/repository/maven_releases/"
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
                      curl -v -u admin:Jose1234 \
			--upload-file target/spring-boot-complete-0.0.1-SNAPSHOT.jar \
			http://host.docker.internal:8081/repository/maven_releases/com/example/spring-boot-complete/0.0.1-SNAPSHOT/spring-boot-complete-0.0.1-SNAPSHOT.jar
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
