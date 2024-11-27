pipeline {
    agent any

    tools {
        maven 'mvn'  // Define Maven tool installation
    }

    stages {
        stage('Hello') {
            steps {
                script {
                    echo 'Hello World'
                }
            }
        }

        stage('Clone Git Repo') {
            steps {
                script {
                    git branch: 'main', url: 'https://github.com/harshithag-9082/maven-app.git'
                }
            }
        }

        stage('Maven Build') {
            steps {
                script {
                    sh 'mvn clean package'
                }
            }
        }

        stage('Create Docker Image') {
            steps {
                script {
                    sh 'docker build -t abhimoly/javamvn .'
                }
            }
        }

        stage('Create Docker Container') {
            steps {
                script {
                    sh 'docker run -d -p 8000:8080 abhimoly/javamvn'
                }
            }
        }
    }

    post {
        always {
            echo 'Pipeline execution finished'
        }
        success {
            echo 'Pipeline executed successfully'
        }
        failure {
            echo 'Pipeline failed'
        }
    }
}
