pipeline {
    agent any

    stages {
        stage('Pull Latest Code') {
            steps {
                git url: 'https://github.com/andreirhamni09/laravel-docker.git', branch: 'master'
            }
        }
        stage('Prepare .env') {
            steps {
                bat 'if not exist app\\.env copy app\\.env.example app\\.env'
            }
        }
        // ganti bat menjadi sh jika yang digunakan adalah os linux begitu juga sebaliknya
        stage('Build Docker') {
            steps {
                bat 'docker-compose down'
                bat 'docker-compose build'
                bat 'docker-compose up -d'
            }
        }

        stage('Run Laravel Commands') {
            steps {
                bat 'docker exec laravel-app composer install'
                bat 'cp app/.env.example app/.env'
                bat 'docker exec laravel-app php artisan migrate --force'
            }
        }
    }
}
