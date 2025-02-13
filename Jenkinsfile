pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
    }

    stages {
        stage('Clone Repository') {
            steps {
                git branch: 'main', url: 'https://gitlab.com/akashkrao01/maven-docker-pipeline.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                script {
                    sh 'docker build -t ${DOCKER_IMAGE} myapp/'  // Build image inside myapp directory
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    sh 'docker run --rm --name ${CONTAINER_NAME} -v $PWD/myapp:/app -w /app ${DOCKER_IMAGE} mvn clean package'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'myapp/target/*.jar', fingerprint: true  // Adjusted path for artifacts
            }
        }

        stage('Execute Post-Build Script') {
            steps {
                script {
                    sh 'bash myapp/post_build.sh'  // Adjusted path for post-build script
                }
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
