pipeline {
    agent any

    environment {
        PATH = "C:\\Windows\\System32;C:\\Rust-Jenkins\\.cargo\\bin;C:\\Rust-Jenkins\\.cargo\\bin;C:\\Rust-Jenkins\\.rustup\\toolchains\\stable-x86_64-pc-windows-msvc\\bin;${env.PATH}"
    }

    stages {

        stage('Rust Debug') {
            steps {
                bat '''
                echo PATH=%PATH%
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