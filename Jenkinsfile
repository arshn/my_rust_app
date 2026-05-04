pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                bat 'cargo build --release'
            }
        }

        stage('Run') {
            steps {
                bat 'target\\release\\my_rust_app.exe'
            }
        }
    }
}