pipeline {
    agent any

    environment {
        // مهم: System32 حتما باید باشه
        PATH = "C:\\Windows\\System32;C:\\Users\\arshn\\.cargo\\bin;${env.PATH}"
    }

    stages {

        stage('Fix Rust Toolchain') {
            steps {
                bat '''
                rustup default stable
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