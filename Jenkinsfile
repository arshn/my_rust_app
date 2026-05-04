pipeline {
    agent any

    stages {
		stage('Test Cargo') {
			steps {
				bat 'cargo --version'
			}
		}

        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                bat 'cargo build --release'
            }
        }
    }
}