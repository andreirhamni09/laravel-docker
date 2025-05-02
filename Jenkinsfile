pipeline {
    agent any

    stages {
        stage('Pull Latest Code') {
            steps {
                git url: 'https://github.com/andreirhamni09/laravel-docker.git', branch: 'master'
            }
        }

        stage('Build Docker') {
            steps {
                sh 'docker-compose down'
                sh 'docker-compose build'
                sh 'docker-compose up -d'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                sh 'docker exec laravel-app composer install'
                sh 'docker exec laravel-app php artisan migrate --force'
            }
        }
    }
}
