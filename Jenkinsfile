pipeline {
    agent any

    environment {
        GITLAB_REPO = "https://gitlab.com/your-username/maven-docker-project.git"
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
        WORKSPACE_DIR = "C:\\Users\\akash\\.jenkins\\workspace\\Maven-Docker-Pipeline\\myapp"
    }

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    bat """
                        if exist myapp (rmdir /s /q myapp)
                        git clone ${GITLAB_REPO} myapp
                    """
                }
            }
        }

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
                script {
                    bat """
                        docker run --rm -v "${WORKSPACE_DIR}:/app" -w "/app" ^
                        ${DOCKER_IMAGE} sh -c "./post_build.sh"
                    """
                }
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
