pipeline {
    agent any

    environment {
        PATH = "C:\\Users\\arshn\\.cargo\\bin;${env.PATH}"
        RUSTUP_TOOLCHAIN = "stable"
        CARGO_HOME = "C:\\Users\\arshn\\.cargo"
        RUSTUP_HOME = "C:\\Users\\arshn\\.rustup"
    }

    stages {
        stage('Debug Rust') {
            steps {
                bat 'rustup show'
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