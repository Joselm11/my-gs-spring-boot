pipeline {
    agent any

    tools {
        maven '3.9.11'
    }

    environment {
        NEXUS_URL = 'http://host.docker.internal:8081/repository/maven-snapshots/'
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                url: 'https://github.com/Joselm11/my-gs-spring-boot.git'
            }
        }

        stage('Build & Deploy to Nexus') {
            steps {
                sh '''
                mvn clean deploy -DskipTests \
                -DaltDeploymentRepository=nexus::default::http://host.docker.internal:8081/repository/maven-snapshots/
                '''
            }
        }

        stage('Verify Artifact') {
            steps {
                sh 'ls -lah target'
            }
        }

        stage('Archive') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }
    }

    post {
        success {
            echo 'Build and deployment SUCCESS!'
        }
        failure {
            echo 'Build FAILED'
        }
    }
}
