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
                checkout scm
            }
        }

        stage('Build Application - Java 17') {
            steps {
                sh 'rm -rf target'
                sh 'docker cp . java17-builder:/workspace'
                sh 'docker exec java17-builder sh -c "cd /workspace && java -version && mvn clean package -DskipTests"'
                sh 'docker cp java17-builder:/workspace/target ./target'
            }
        }

        stage('Run Unit Tests - Java 11') {
            steps {
                sh 'docker cp . java11-tester:/workspace'
                sh 'docker exec java11-tester sh -c "cd /workspace && java -version && mvn test"'
            }
        }

        stage('Analyze Code - SonarQube') {
            steps {
                withSonarQubeEnv("${SONARQUBE_SERVER}") {
                    sh 'docker cp . java11-tester:/workspace'
                    sh 'docker exec java11-tester sh -c "cd /workspace && java -version && mvn sonar:sonar -Dsonar.projectKey=java-app -Dsonar.projectName=java-app -Dsonar.host.url=http://sonarqube:9000 -Dsonar.login=$SONAR_AUTH_TOKEN"'
                }
            }
        }

        stage('Build Docker Image') {
            steps {
                sh 'ls -la target'
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
        sh 'kubectl --kubeconfig=/root/.kube/config apply -f k8s/deployment.yaml --validate=false'
            }
        }

    }
}
