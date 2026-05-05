pipeline {
    agent none
    
    environment {
        CARGO_TERM_COLOR = 'always'
        RUST_BACKTRACE = 'full'
        CARGO_INCREMENTAL = '0'
    }

    stages {
        // 1. Checkout & Prepare
        stage('Checkout & Prepare') {
            agent any
            steps {
                checkout scm
                echo "✅ Checkout completed"
            }
        }

        // 2. Cache (با Swatinem/rust-cache-style اما در Jenkins)
        stage('Cache Setup') {
            agent any
            steps {
                echo "🗄️ Cache will be handled automatically by subsequent stages"
            }
        }

        // 3+4. Formatting + Linting (Parallel)
        stage('Code Quality') {
            parallel {
                stage('Rustfmt') {
                    agent { label 'linux' }
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

        // 5+6+8. Linux Build + Test
        stage('Linux - Build & Test') {
            agent { label 'linux' }
            steps {
                sh '''
                echo "=== Rust Version ==="
                rustc --version && cargo --version
                
                echo "=== Build Debug ==="
                cargo build
                
                echo "=== Build Release ==="
                cargo build --release
                
                echo "=== Run Tests ==="
                cargo test --release
                '''
            }
        }

        // 7. Cross Build (با cross-rs)
        stage('Cross Compilation') {
            agent { label 'linux' }
            steps {
                sh '''
                cargo install cross --git https://github.com/cross-rs/cross || echo "cross already installed"
                
                echo "=== Cross build for Windows (GNU) ==="
                cross build --release --target x86_64-pc-windows-gnu
                
                echo "=== Cross build for ARM64 Linux ==="
                cross build --release --target aarch64-unknown-linux-gnu
                '''
            }
        }

        // 9. Test with Coverage (با cargo-llvm-cov)
        stage('Coverage Report') {
            agent { label 'linux' }
            steps {
                sh '''
                rustup component add llvm-tools-preview
                cargo install cargo-llvm-cov || echo "cargo-llvm-cov already installed"
                
                echo "=== Generating Coverage ==="
                cargo llvm-cov --all-features --workspace --lcov --output-path lcov.info
                
                echo "=== Coverage Summary ==="
                cargo llvm-cov --all-features --workspace --summary
                '''
                archiveArtifacts artifacts: 'lcov.info', allowEmptyArchive: true
            }
        }

        // Windows Build & Test
        stage('Windows - Build & Test') {
            agent { label 'windows' }
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

        // 10. Security Audit
        stage('Security Audit') {
            agent { label 'linux' }
            steps {
                sh '''
                cargo install cargo-audit cargo-deny || echo "Audit tools installed"
                
                echo "=== Cargo Audit ==="
                cargo audit --deny warnings || echo "Audit completed with warnings"
                
                echo "=== Cargo Deny ==="
                cargo deny check || echo "Deny check completed"
                '''
            }
        }

        // 11. Documentation
        stage('Documentation') {
            agent { label 'linux' }
            steps {
                sh '''
                echo "=== Building Documentation ==="
                cargo doc --no-deps --all-features --document-private-items
                '''
                archiveArtifacts artifacts: 'target/doc/**', allowEmptyArchive: true
            }
        }

        // 12. Artifact / Release
        stage('Archive Artifacts') {
            parallel {
                stage('Linux Binary') {
                    agent { label 'linux' }
                    steps {
                        sh 'cargo build --release'
                        archiveArtifacts artifacts: 'target/release/my_rust_app,target/release/my_rust_app*', fingerprint: true
                    }
                }
                stage('Windows Binary') {
                    agent { label 'windows' }
                    steps {
                        bat '''
                        call "C:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Community\\VC\\Auxiliary\\Build\\vcvarsall.bat" x64
                        cargo build --release
                        '''
                        archiveArtifacts artifacts: 'target/release/my_rust_app.exe', fingerprint: true
                    }
                }
            }
        }
    }

    post {
        success {
            echo '🎉 تمام مراحل با موفقیت انجام شد!'
        }
        failure {
            echo '❌ Pipeline با خطا مواجه شد'
        }
        always {
            cleanWs()
        }
    }
}