pipeline {
    agent any

    environment {
        IMAGE_NAME = "mon-site-php"
        IMAGE_TAG  = "build-${env.BUILD_NUMBER}"
    }

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/thiothiod/test_php.git'
            }
        }

        stage('Build Docker Image') {
            steps {
                sh "docker build -t ${IMAGE_NAME}:${IMAGE_TAG} ."
            }
        }

        stage('Vérifier la version PHP') {
            steps {
                sh "docker run --rm ${IMAGE_NAME}:${IMAGE_TAG} php -v"
            }
        }

        stage('Vérification de la syntaxe') {
            steps {
                sh """
                    docker run --rm \
                        -v \$(pwd):/var/www/html \
                        ${IMAGE_NAME}:${IMAGE_TAG} \
                        find /var/www/html -name '*.php' -exec php -l {} \\;
                """
            }
        }

        stage('Installation des dépendances') {
            steps {
                sh """
                    docker run --rm \
                        -v \$(pwd):/var/www/html \
                        ${IMAGE_NAME}:${IMAGE_TAG} \
                        composer install --no-interaction --prefer-dist
                """
            }
        }

        stage('Exécution des tests') {
            steps {
                sh """
                    docker run --rm \
                        -v \$(pwd):/var/www/html \
                        ${IMAGE_NAME}:${IMAGE_TAG} \
                        ./vendor/bin/phpunit --testdox
                """
            }
        }

    }

    post {
        always {
            sh "docker rmi ${IMAGE_NAME}:${IMAGE_TAG} || true"
        }
        success {
            echo '✅ Pipeline réussi !'
        }
        failure {
            echo '❌ Pipeline échoué. Vérifiez les logs.'
        }
    }
}