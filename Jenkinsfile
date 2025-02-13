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
                    sh 'docker build -t ${DOCKER_IMAGE} .'
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    sh 'docker run --name ${CONTAINER_NAME} ${DOCKER_IMAGE}'
                }
            }
        }

        stage('Archive Artifacts') {
            steps {
                archiveArtifacts artifacts: 'target/*.jar', fingerprint: true
            }
        }

        stage('Execute Post-Build Script') {
            steps {
                script {
                    sh 'bash post_build.sh'
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
