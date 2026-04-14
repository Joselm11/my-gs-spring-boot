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
                configFileProvider([configFile(fileId: 'f59eeba7-c514-41b6-b32d-80d6565f70b3', variable: 'MAVEN_SETTINGS')]) {
    		sh 'mvn clean deploy -DskipTests -s $MAVEN_SETTINGS'
		}
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
