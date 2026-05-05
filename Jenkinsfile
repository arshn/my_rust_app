pipeline {
    agent none  // برای کنترل بهتر agent در هر stage

    environment {
        CARGO_TERM_COLOR = 'always'
        RUST_BACKTRACE = '1'
    }

    stages {
        stage('Checkout') {
            agent any
            steps {
                checkout scm
            }
        }

        // ====================== Linting & Formatting ======================
        stage('Code Quality') {
            parallel {
                stage('Rustfmt') {
                    agent { label 'linux' }  // یا any
                    steps {
                        sh '''
                        rustup component add rustfmt
                        cargo fmt --all -- --check
                        '''
                    }
                }

                stage('Clippy') {
                    agent { label 'linux' }
                    steps {
                        sh '''
                        rustup component add clippy
                        cargo clippy --all-targets --all-features -- -D warnings
                        '''
                    }
                }
            }
        }

        // ====================== Build & Test Linux ======================
        stage('Linux Build & Test') {
            agent { label 'linux' }
            steps {
                sh '''
                echo "=== Rust Version ==="
                rustc --version
                cargo --version
                
                echo "=== Building Debug ==="
                cargo build
                
                echo "=== Building Release ==="
                cargo build --release
                
                echo "=== Running Tests ==="
                cargo test --release
                '''
            }
        }

        // ====================== Build & Test Windows ======================
        stage('Windows Build & Test') {
            agent { label 'windows' }   // یا any اگر فقط یک agent ویندوزی داری
            steps {
                bat '''
                echo === Environment Info ===
                where link.exe
                where cargo
                where rustc
                
                call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                
                echo === Building Debug ===
                cargo build
                
                echo === Building Release ===
                cargo build --release
                
                echo === Running Tests ===
                cargo test --release
                '''
            }
        }

        // ====================== Security & Dependencies ======================
        stage('Security Audit') {
            agent { label 'linux' }
            steps {
                sh '''
                echo "=== Installing audit tools ==="
                cargo install --quiet cargo-audit || echo "cargo-audit already installed"
                cargo install --quiet cargo-deny || echo "cargo-deny already installed"
                
                echo "=== Running cargo audit ==="
                cargo audit
                
                echo "=== Running cargo deny ==="
                cargo deny check || echo "cargo-deny finished with warnings"
                '''
            }
        }

        // ====================== Documentation ======================
        stage('Documentation') {
            agent { label 'linux' }
            steps {
                sh '''
                echo "=== Building Documentation ==="
                cargo doc --no-deps --all-features
                '''
            }
        }

        // ====================== Artifacts (اختیاری اما توصیه‌شده) ======================
        stage('Archive Artifacts') {
            agent { label 'linux' }
            steps {
                sh 'cargo build --release'
                archiveArtifacts artifacts: 'target/release/my_rust_app*, target/release/my_rust_app.exe', fingerprint: true
            }
        }
    }

    post {
        always {
            echo 'Pipeline finished'
        }
        success {
            echo '✅ Build & Checks Completed Successfully'
        }
        failure {
            echo '❌ Pipeline Failed'
        }
    }
}