pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
        WORKSPACE_DIR = "C:\\Users\\akash\\.jenkins\\workspace\\Maven-Docker-Pipeline\\myapp"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    bat """
                        docker build -t ${DOCKER_IMAGE} myapp
                    """
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    bat """
                        docker run --rm --name ${CONTAINER_NAME} ^
                        -v "${WORKSPACE_DIR}:/app" -w "/app" ^
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
                bat 'echo "Build completed successfully"'
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
