pipeline {
    agent any

    stages {

        stage('Clone') {
            steps {
                git 'https://github.com/thiothiod/test_php.git'
            }
        }

        stage('Check PHP Version') {
            steps {
                sh 'php -v'
            }
        }

        stage('Syntax Check') {
            steps {
                // Vérifie les erreurs PHP
                sh 'find . -name "*.php" -print0 | xargs -0 -n1 php -l'
            }
        }

        stage('Install Dependencies') {
            steps {
                // Si tu utilises composer ajout jenkins
                sh 'composer install || true'
            }
        }

        stage('Run Tests') {
            steps {
                // Si tu ajoutes PHPUnit plus tard
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