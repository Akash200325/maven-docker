pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "maven-docker-build"
        CONTAINER_NAME = "maven_build_container"
        WORKSPACE_PATH = "C:/Users/akash/.jenkins/workspace/Maven-Docker-Pipeline/myapp"
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
                    sh 'docker build -t maven-docker-build myapp/'  
                }
            }
        }

        stage('Run Container and Build Maven Project') {
            steps {
                script {
                    sh '''
                    docker run --rm --name maven_build_container \
                    -v "C:/Users/akash/.jenkins/workspace/Maven-Docker-Pipeline/myapp:/app" \
                    -w "/app" maven-docker-build mvn clean package
                    '''
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
                    sh 'bash myapp/post_build.sh'
                }
            }
        }
    }

    post {
        success {
            echo "✅ Pipeline executed successfully!"
        }
        failure {
            echo "❌ Pipeline failed! Check logs for details."
        }
    }
}
