pipeline {
    agent any

    stages {
        stage('1. Ambil Kode') {
            steps {
                echo 'Mengambil kode terbaru dari GitHub adek...'
                checkout scm
            }
        }

        stage('2. Cek Lingkungan') {
            steps {
                echo 'Mengecek apakah PHP dan Composer sudah siap...'
                sh 'php -v'
                sh 'composer -v'
            }
        }

        stage('3. Install Dependencies') {
            steps {
                echo 'Menginstall library Laravel (Composer Install)...'
                // Di sini Jenkins akan mendownload semua yang dibutuhkan Laravel
                sh 'composer install --no-interaction --prefer-dist --optimize-autoloader'
            }
        }

        stage('4. Persiapan Aplikasi') {
            steps {
                echo 'Menyiapkan file .env dan App Key...'
                sh 'cp .env.example .env'
                sh 'php artisan key:generate'
            }
        }

        stage('5. Testing') {
            steps {
                echo 'Menjalankan unit test... Bismillah Hijau!'
                sh 'php artisan test'
            }
        }
    }
    
    post {
        success {
            echo 'ALHAMDULILLAH! Project adek berhasil di-build dan test lolos semua! 🎉'
        }
        failure {
            echo 'Waduh, ada yang error dek. Cek log-nya ya! ❌'
        }
    }
}