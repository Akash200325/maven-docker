pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} myapp'
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    // Convert Jenkins WORKSPACE path for Windows Docker
                    def dockerWorkspace = WORKSPACE.replaceAll('\\\\', '/')  // Convert \ to /
                    
                    sh """
                        docker run --rm --name ${CONTAINER_NAME} ^
                        -v "${dockerWorkspace}/myapp:C:/app" -w "C:/app" ^
                        ${DOCKER_IMAGE} sh -c "mvn clean package"
                    """
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'myapp/target/*.jar', fingerprint: true
            }
        }

        stage('Execute Post-Build Script') {
            steps {
                sh 'echo "Build completed successfully"'
            }
        }
    }

    post {
        failure {
            echo "❌ Pipeline failed! Check logs for details."
        }
        success {
            echo "✅ Pipeline executed successfully!"
        }
    }
}
