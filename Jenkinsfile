pipeline {
    agent any   // استفاده از agent فعلی (ویندوز)

    environment {
        CARGO_TERM_COLOR = 'always'
        RUST_BACKTRACE = 'full'
    }

    stages {
        stage('Checkout & Prepare') {
            steps {
                checkout scm
                echo "✅ Checkout completed"
            }
        }

        stage('Code Quality') {
            parallel {
                stage('Rustfmt') {
                    steps {
                        bat 'rustup component add rustfmt'
                        bat 'cargo fmt --all -- --check'
                    }
                }
                stage('Clippy') {
                    steps {
                        bat 'rustup component add clippy'
                        bat 'cargo clippy --all-targets --all-features -- -D warnings'
                    }
                }
            }
        }

        stage('Build & Test') {
            steps {
                bat '''
                call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                
                echo === Build Debug ===
                cargo build
                
                echo === Build Release ===
                cargo build --release
                
                echo === Run Tests ===
                cargo test --release
                '''
            }
        }

        stage('Security Audit') {
            steps {
                bat '''
                cargo install cargo-audit --quiet || echo "cargo-audit installed"
                cargo audit
                '''
            }
        }

        stage('Documentation') {
            steps {
                bat 'cargo doc --no-deps --all-features'
            }
        }

        stage('Archive Artifacts') {
            steps {
                bat '''
                call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                cargo build --release
                '''
                archiveArtifacts artifacts: 'target/release/my_rust_app.exe', fingerprint: true, allowEmptyArchive: true
            }
        }
    }

    post {
        success {
            echo '🎉 تمام مراحل با موفقیت انجام شد!'
        }
        failure {
            echo '❌ Pipeline Failed'
        }
    }
}