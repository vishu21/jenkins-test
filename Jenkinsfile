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
                git branch: "${params.BRANCH_NAME}", credentialsId: 'github-pat', url: 'https://github.com/vishu21/jenkins-test.git'
            }
        }

        stage('Build') {
            steps {
                echo "Building ${params.ENV} for ${env.PROJECT_NAME}"
            }
        }

        stage('Conditional Test') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                echo "Running tests for ${env.PROJECT_NAME}..."
            }
        }

        stage('Tests in Parallel') {
            when {
                expression { return params.RUN_TESTS }
            }
            steps {
                script {
                    parallel(
                        'Unit Tests': {
                            echo 'Running unit tests...'
                        },
                        'Integration Tests': {
                            echo 'Running integration tests...'
                        }
                    )
                }
            }
        }

        stage('Approval') {
            steps {
                input message: 'Deploy to production?', ok: 'Yes, deploy!'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying to production...'
            }
        }
    }

    post {
        success {
            echo '✅ Build completed successfully!'
            mail to: 'vbrar6076@gmail.com',
                 subject: "Jenkins Job completed: ${env.PROJECT_NAME} [${env.BUILD_NUMBER}]",
                 body: "Check the console output at ${env.BUILD_URL}"
        }
        failure {
            echo '❌ Build failed!'
        }
    }
}
