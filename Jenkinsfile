pipeline {
    agent any

    environment {
        PATH = "C:\\Users\\arshn\\.cargo\\bin;C:\\Program Files\\Git\\bin;C:\\Windows\\System32"
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Check Rust') {
            steps {
                bat 'rustc --version'
                bat 'cargo --version'
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
}