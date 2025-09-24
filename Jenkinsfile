pipeline {
    agent any

    tools {
        jdk 'jdk17'
        maven 'maven3'
    }

    environment {
        APP_JAR = "target/mavenapp-1.0.0.jar"
        APP_PORT = "8081"
    }

    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/KishorKP22/Jenkins-CICD-Java-Maven-Project.git'
            }
        }
        stage('Build & Package') {
            steps {
                sh 'mvn clean package'
            }
        }
        stage('Run Tests') {
            steps {
                sh 'mvn test'
            }
        }
        stage('Deploy') {
            steps {
                sh '''
                pkill -f "java -jar $APP_JAR" || true
                nohup java -jar $APP_JAR --server.port=$APP_PORT > /var/log/mavenapp.log 2>&1 &
                '''
            }
        }
    }
    post {
        always {
            archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
        }
    }
}
