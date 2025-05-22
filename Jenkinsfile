
pipeline {
    agent any

    parameters {
        string(name: 'BRANCH_NAME', defaultValue: 'main', description: 'Git branch to build')
        choice(name: 'ENV', choices: ['dev', 'staging', 'prod'], description: 'Select environment')
        booleanParam(name: 'RUN_TESTS', defaultValue: true, description: 'Run unit tests?')
    }

    environment {
        PROJECT_NAME = "jenkins-test"
    }

    stages {
        stage('Clone Repo') {
            steps {
                git branch: "${prams.BRANCH_NAME}", credentialsId: 'github-pat', url: 'https://github.com/vishu21/jenkins-test.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${params.ENV} for ${env.PROJECT_NAME}"
            }
        }

        stage('Test') {
            when {
                expression: { return params.RUN_TESTS }
            }
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
