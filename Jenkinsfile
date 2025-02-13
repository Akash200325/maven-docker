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
                        echo "Building Docker image..."
                        docker build -t %DOCKER_IMAGE% myapp
                    """
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    bat """
                        echo "Running Maven build inside Docker container..."
                        docker run -d --name %CONTAINER_NAME% ^
                        -v "%WORKSPACE_DIR%:/app" -w "/app" ^
                         %DOCKER_IMAGE% sh -c "mvn clean package"
                    """
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                script {
                    bat """
                        if exist myapp\\target\\*.jar (
                            echo " Archiving build artifacts..."
                        ) else (
                            echo " No JAR file found, build may have failed!"
                            exit /b 1
                        )
                    """
                    archiveArtifacts artifacts: 'myapp/target/*.jar', fingerprint: true
                }
            }
        }

        stage('Execute Post-Build Script') {
            steps {
                script {
                    bat """
                        echo "Build completed successfully!"
                        if exist myapp\\post_build.sh (
                            sh myapp/post_build.sh
                        ) else (
                            echo " No post-build script found!"
                        )
                    """
                }
            }
        }
    }

    post {
        failure {
            echo "Pipeline failed! Check logs for details."
        }
        success {
            echo "Pipeline executed successfully!"
        }
    }
}
