pipeline {
    agent any

    stages {

        stage('Fix PATH') {
            steps {
                bat '''
                set PATH=C:\\Windows\\System32;C:\\Windows;C:\\Users\\arshn\\.cargo\\bin;%PATH%
                '''
            }
        }

        stage('Rust Debug') {
            steps {
                bat '''
                where cmd
                where cargo
                where rustc
                cargo --version
                rustc --version
                '''
            }
        }

        stage('Fix Rust Toolchain') {
            steps {
                bat 'rustup default stable'
            }
        }

        stage('Build') {
            steps {
                bat 'cargo build --release'
            }
        }

        stage('Test') {
            steps {
                bat 'cargo test'
            }
        }
    }

    post {
        success {
            echo "✅ Build OK"
        }
        failure {
            echo "❌ Build Failed"
        }
    }
}