pipeline {
    agent any

    tools {
            maven 'MVN' // Replace 'Maven' with the name of the Maven installation in Jenkins
        }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins
        SONAR_TOKEN = 'sqa_fa4b690c52006cb1f69b4e06885255320f557366' // Store the token securely
        DOCKERHUB_CREDENTIALS_ID = 'dockercred'
        DOCKERHUB_REPO = 'jess3/inclassassignmentweek5'
        DOCKER_IMAGE_TAG = 'latest_v1'
        PATH = "/usr/local/bin:$PATH"
    }


    stages {
        stage('Checkout') {
            steps {
                git branch: 'master', url: 'https://github.com/J3-ss3/InclassAssignmentWeek5.git'
            }
        }

        stage('Build') {
            steps {
                sh 'mvn clean install'
            }
        }

        stage('Debug PATH') {
                steps {
                    sh 'echo $PATH'
                }
            }

        stage('SonarQube Analysis') {
            steps {
                withSonarQubeEnv('SonarQubeServer') {
                    sh """
                                    sonar-scanner \
                                    -Dsonar.projectKey=devops-demo \
                                    -Dsonar.sources=src \
                                    -Dsonar.projectName=DevOps-Demo \
                                    -Dsonar.host.url=http://localhost:9000 \
                                    -Dsonar.login=${SONAR_TOKEN} \
                                    -Dsonar.java.binaries=target/classes
                                """
                    }
            }
        }
        stage('Build Docker Image') {
                            steps {
                                // Build Docker image
                                script {
                                    docker.build("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}")
                                }
                            }
                        }
                        stage('Push Docker Image to Docker Hub') {
                            steps {
                                // Push Docker image to Docker Hub
                                script {
                                    docker.withRegistry('https://index.docker.io/v1/', DOCKERHUB_CREDENTIALS_ID) {
                                        docker.image("${DOCKERHUB_REPO}:${DOCKER_IMAGE_TAG}").push()
                                    }
                                }
                            }
                        }
    }
}
