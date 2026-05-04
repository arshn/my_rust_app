pipeline {
    agent any

    environment {
        CARGO_HOME = "C:\\Users\\arshn\\.cargo"
        PATH = "C:\\Users\\arshn\\.cargo\\bin;C:\\Program Files\\Git\\cmd;C:\\Windows\\System32"
    }

    stages {

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Rust Check') {
            steps {
                bat 'where cargo'
                bat 'where rustc'
                bat 'cargo --version'
                bat 'rustc --version'
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
            echo '✅ Build successful'
        }
        failure {
            echo '❌ Build failed'
        }
    }
}