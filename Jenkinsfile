pipeline {
    agent any

    tools {
            maven 'MVN' // Replace 'Maven' with the name of the Maven installation in Jenkins
        }

    environment {
        SONARQUBE_SERVER = 'SonarQubeServer'  // The name of the SonarQube server configured in Jenkins
        SONAR_TOKEN = 'sqa_fa4b690c52006cb1f69b4e06885255320f557366' // Store the token securely
    }

    stage('Debug PATH') {
        steps {
            sh 'echo $PATH'
        }
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
    }
}
