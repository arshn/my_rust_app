pipeline {
    agent any

    environment {
        PATH = "C:\\Users\\arshn\\.cargo\\bin;${env.PATH}"
    }

    stages {

        stage('Rust Debug') {
            steps {
                bat '''
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