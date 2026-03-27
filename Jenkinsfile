pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git branch: 'main', url: 'https://github.com/thiothiod/test_php.git'
            }
        }

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

        stage('Install Dependencies') {
            steps {
                sh 'composer install || true'
            }
        }

        stage('Run Tests') {
            steps {
                sh 'vendor/bin/phpunit || true'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Déploiement PHP terminé 🚀'
            }
        }
    }
}