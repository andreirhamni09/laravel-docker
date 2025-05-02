pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                git url: 'https://github.com/andreirhamni09/laravel-docker.git', branch: 'master'
            }
        }

        stage('Cleanup Docker (Optional)') {
            steps {
                // Stop and remove all matching containers (if exists)
                bat '''
                docker stop laravel-app || exit 0
                docker stop mysql || exit 0
                docker stop nginx || exit 0
                docker rm laravel-app || exit 0
                docker rm mysql || exit 0
                docker rm nginx || exit 0
                docker volume rm laravel-docker_dbdata || exit 0
                '''
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
                bat 'docker-compose down --remove-orphans'
                bat 'docker-compose build --no-cache'
                bat 'docker-compose up -d'
            }
        }

        stage('Fix File Permissions') {
            steps {
                bat 'timeout /t 5'
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
