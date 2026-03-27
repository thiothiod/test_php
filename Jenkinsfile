pipeline {
    agent any

    environment {
        REGISTRY_URL = "192.168.1.17:5001"
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
                sh "docker build -t ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Push to Registry') {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'docker-credentials', // créé dans Jenkins avec ton user/pass
                    usernameVariable: 'REGISTRY_USER',
                    passwordVariable: 'REGISTRY_PASS'
                )]) {
                    sh "docker login ${REGISTRY_URL} -u ${REGISTRY_USER} -p ${REGISTRY_PASS}"
                    sh "docker push ${REGISTRY_URL}/${IMAGE_NAME}:${IMAGE_TAG}"
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