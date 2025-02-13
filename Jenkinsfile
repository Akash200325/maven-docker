pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
        WORK_DIR = "/app"
        WINDOWS_PATH = "${WORKSPACE}\\myapp"
        UNIX_PATH = "/c/Users/akash/.jenkins/workspace/Maven-Docker-Pipeline/myapp"
    }

    stages {
        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    def mountPath = isUnix() ? "${WORKSPACE}/myapp" : "${UNIX_PATH}"
                    sh """
                        docker run --rm --name ${CONTAINER_NAME} \
                        -v "${mountPath}:${WORK_DIR}" -w ${WORK_DIR} \
                        ${DOCKER_IMAGE} mvn clean package
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
