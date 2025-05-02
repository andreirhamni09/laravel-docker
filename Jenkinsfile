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
                cat 'docker-compose down'
                cat 'docker-compose build'
                cat 'docker-compose up -d'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                cat 'docker exec laravel-app composer install'
                cat 'docker exec laravel-app php artisan migrate --force'
            }
        }
    }
}
