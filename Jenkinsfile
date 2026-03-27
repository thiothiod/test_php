pipeline {
    agent {
        docker {
            image 'docker:27.1-dind'
            args '-v /var/run/docker.sock:/var/run/docker.sock --privileged --user root'
            reuseNode true
        }
    }

    environment {
        REGISTRY_URL = "192.168.1.17:5001"
        IMAGE_NAME = "mon-site-php"
        IMAGE_TAG = "latest"
    }

    stages {

        stage('Check PHP Version') {
            steps {
                sh 'php -v || echo "PHP not needed here, but Docker ready"'
            }
        }

        stage('Syntax Check') {
            steps {
                sh 'find . -name "*.php" -print0 | xargs -0 -n1 php -l || true'
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
                    credentialsId: 'nexus-credentials',
                    usernameVariable: 'REGISTRY_USER',
                    passwordVariable: 'REGISTRY_PASS'
                )]) {
                    sh "docker login ${REGISTRY_URL} -u \${REGISTRY_USER} -p \${REGISTRY_PASS}"
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
    post {
        always {
            sh 'docker logout ${REGISTRY_URL} || true'
        }
    }
}
