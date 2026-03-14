pipeline {
    agent any

    environment {
        DOCKER_IMAGE = "zero0x00/java-app:${BUILD_NUMBER}"
        DOCKER_LATEST = "zero0x00/java-app:latest"
        DOCKER_CREDENTIALS_ID = "dockerhub-creds"
        SONARQUBE_SERVER = "sonarqube-local"
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/Zero0x00/CYB-535.git'
            }
        }

        stage('Build Application - Java 17') {
            steps {
                sh 'docker cp . java17-builder:/workspace'
                sh 'docker exec java17-builder sh -c "cd /workspace && java -version && mvn clean package -DskipTests"'
            }
        }

        stage('Run Unit Tests - Java 11') {
            steps {
                sh 'docker cp . java11-tester:/workspace'
                sh 'docker exec java11-tester sh -c "cd /workspace && java -version && mvn test"'
            }
        }

        stage('Analyze Code - SonarQube with Java 8') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'docker cp . java8-analyzer:/workspace'
                    sh 'docker exec java8-analyzer sh -c "cd /workspace && java -version && mvn sonar:sonar -Dsonar.projectKey=java-app -Dsonar.projectName=java-app -Dsonar.host.url=$SONAR_HOST_URL -Dsonar.login=$SONAR_AUTH_TOKEN"'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'docker build -t ${DOCKER_IMAGE} -t ${DOCKER_LATEST} .'
            }
        }

        stage('Push Docker Image') {
            steps {
                withDockerRegistry([credentialsId: "${DOCKER_CREDENTIALS_ID}", url: ""]) {
                    sh 'docker push ${DOCKER_IMAGE}'
                    sh 'docker push ${DOCKER_LATEST}'
                }
            }
        }

        stage('Deploy to Kubernetes') {
            steps {
                sh 'kubectl apply -f deployment.yaml'
            }
        }
    }
}
