pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/andreirhamni09/laravel-docker.git', branch: 'master'
            }
        }

        stage('Prepare .env') {
            steps {
                bat 'if not exist app\\.env copy app\\.env.example app\\.env'
                bat 'icacls app\\.env /grant Everyone:F'
            }
        }

        stage('Build and Start Docker') {
            steps {
                bat 'docker-compose down'
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }

        stage('Fix File Permissions') {
            steps {
                bat 'docker exec laravel-app chmod 664 /var/www/.env'
            }
        }

        stage('Laravel Setup') {
            steps {
                bat 'docker exec laravel-app composer install'
                bat 'docker exec laravel-app php artisan key:generate'
                bat 'docker exec laravel-app php artisan migrate --force'
            }
        }
    }
}
