pipeline {
    agent any

    environment {
        PATH = "C:\\Users\\arshn\\.cargo\\bin;${env.PATH}"
        RUSTUP_TOOLCHAIN = "stable"
    }

    stages {
        stage('Debug Rust') {
            steps {
                bat 'where rustc'
                bat 'where cargo'
                bat 'rustc --version'
                bat 'cargo --version'
            }
        }

        stage('Build') {
            steps {
                bat 'cargo build --release'
            }
        }
    }
}