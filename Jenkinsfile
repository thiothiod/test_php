pipeline {
    agent any

    stages {

        stage('Cloner') {
            steps {
                git branch: 'main', url: 'https://github.com/thiothiod/test_php.git'
            }
        }

        stage('Verifier la version PHP') {
            steps {
                sh 'php -v'
            }
        }

        stage('Verification de la syntaxe') {
            steps {
                sh 'find . -name "*.php" -print0 | xargs -0 -n1 php -l'
            }
        }

        stage('Installation des dependances') {
            steps {
                sh 'composer install --no-interaction --prefer-dist'
            }
        }

        stage('Execution des tests') {
            steps {
                sh 'vendor/bin/phpunit --testdox'
            }
        }

        stage('Deploiement') {
            steps {
                echo 'Deploiement PHP termine !'
            }
        }
    }

    post {
        success {
            echo 'Pipeline reussi !'
        }
        failure {
            echo 'Pipeline echoue. Verifiez les logs.'
        }
    }
}