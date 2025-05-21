
pipeline {
    agent any

    environment {
        PROJECT_NAME = "My Jenkins Demo"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: 'main', credentialsId: 'github-pat', url: 'https://github.com/vishu21/jenkins-test.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${env.PROJECT_NAME}..."
            }
        }

        stage('Test') {
            steps {
                echo "Running tests for ${env.PROJECT_NAME}..."
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
