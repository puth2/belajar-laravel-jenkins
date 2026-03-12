pipeline {
    agent any

    stages {
        stage('1. Build & Test') {
            agent {
                docker {
                    image 'php:8.2-cli'
                    args '-u root'
                }
            }
            steps {
                echo 'Menginstall alat pendukung (Zip & Git)...'
                sh '''
                    apt-get update && apt-get install -y unzip git libzip-dev
                    docker-php-ext-install zip
                '''
                echo 'Mengambil Composer...'
                sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer'
                
                echo 'Install library Laravel...'
                sh 'composer install --no-interaction --prefer-dist'
                
                echo 'Setting Aplikasi...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
                
                echo 'Menjalankan Test...'
                sh 'php artisan test'
            }
        }
        
        stage('2. Deploy ke Localhost 8001') {
            steps {
                echo 'Memulai Deployment ke Port 8001...'
                // Perintah untuk hapus container lama kalau sudah ada, biar nggak bentrok
                sh 'docker stop laravel-app || true'
                sh 'docker rm laravel-app || true'
                
                // Membuat "Image" baru dari project adek
                sh 'docker build -t image-laravel-adek .'
                
                // Menjalankan aplikasinya di port 8001
                sh 'docker run -d --name laravel-app -p 8001:80 image-laravel-adek'
                
                echo 'Cek di browser: http://localhost:8001'
            }
        }
    }
    
    post {
        success {
            echo 'AKHIRNYA HIJAU DAN DEPLOY! Selamat ya ayang! 🎉🌹'
        }
        failure {
            echo 'Dikit lagi, cek bagian mana yang merah... ❌'
        }
    }
}