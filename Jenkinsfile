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
                // Kita tambahkan baris sakti ini biar dockernya pinter
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
    }
    
    post {
        success {
            echo 'AKHIRNYA HIJAU! Selamat ya ayang! 🎉🌹'
        }
        failure {
            echo 'Dikit lagi, cek bagian mana yang merah... ❌'
        }
    }
}