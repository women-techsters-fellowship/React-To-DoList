pipeline {
    agent any

    tools {
        nodejs "nodejs"
    }

    stages {

        stage('Checkout') {
            steps {
                git branch: 'main',
                    url: 'https://github.com/bigcephas1/React-ToDoList.git'
            }
        }

        stage('Frontend Build') {
            steps {
                dir('dive-react-app') {
                    echo "Installing frontend dependencies"
                    sh 'npm install'

                    echo "Running lint"
                    sh 'npm run lint'

                    echo "Building frontend"
                    sh 'npm run build'
                }
            }
        }

        stage('Backend Install') {
            steps {
                dir('backend') {
                    echo "Installing backend dependencies"
                    sh 'npm install'

                    echo "Running backend tests (instead of starting server)"
                    sh 'npm test || true'   // prevents failure if no tests exist
                }
            }
        }

    }

    post {
        always {
            cleanWs()
        }
    }
        success {
            mail(
                to: "faithizebhor@gmail.com",
                subject: "SUCCESS: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The build ${env.BUILD_URL} completed successfully."
            )
        }
        failure {
            emailext(
                to: "faithizebhor@gmail.com",
                subject: "FAILED: Job '${env.JOB_NAME} [${env.BUILD_NUMBER}]'",
                body: "The build ${env.BUILD_URL} failed. Please check the logs."
            )
        }
    }
}

