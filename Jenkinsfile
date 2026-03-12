pipeline {
    agent any

    stages {
        stage('1. Persiapan Lingkungan') {
            steps {
                echo 'Meminjam Docker PHP untuk memasak...'
            }
        }

        stage('2. Build & Test') {
            agent {
                docker {
                    image 'php:8.2-cli' // Jenkins otomatis pinjam PHP 8.2
                    args '-u root'
                }
            }
            steps {
                echo 'Mengecek alat tempur...'
                sh 'php -v'
                
                echo 'Mengambil Composer...'
                // Perintah ini buat download composer di dalam docker tadi
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
            echo 'ALHAMDULILLAH! Berhasil Hijau, Sayang! 🎉'
        }
        failure {
            echo 'Masih ada yang kurang, cek log di atas ya! ❌'
        }
    }
}