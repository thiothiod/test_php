pipeline {
    agent any

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

        stage('Deploy') {
            steps {
                echo 'Déploiement PHP terminé 🚀'
            }
        }
    }
}