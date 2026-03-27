pipeline {
    agent any

    environment {
        NEXUS_URL = "192.168.1.17:8082"
        IMAGE_NAME = "mon-site-php"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Check PHP Version') {
            steps {
                sh 'php -v'
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'find . -name "*.php" -print0 | xargs -0 -n1 php -l'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${NEXUS_URL}/${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Nexus') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'nexus-credentials',
                    usernameVariable: 'NEXUS_USER',
                    passwordVariable: 'NEXUS_PASS'
                )]) {
                    sh "docker login ${NEXUS_URL} -u ${NEXUS_USER} -p ${NEXUS_PASS}"
                    sh "docker push ${NEXUS_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
                }
            }
        }

        stage('Deploy') {
            steps {
                echo 'Déploiement PHP terminé 🚀'
            }
        }
    }
}