pipeline {
    agent { label 'student-code' }

    environment {
        BUILD_DIR = 'build'
    }

    stages {

        stage('Build') {
            steps {
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y cmake build-essential gcc
                    mkdir -p ${BUILD_DIR}
                    cd ${BUILD_DIR}
                    cmake ..
                    cmake --build .
                '''
            }
            post {
                success {
                    archiveArtifacts artifacts: '${BUILD_DIR}/**', fingerprint: true
                }
            }
        }

        stage('Test') {
            steps {
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y cmake
                    cd ${BUILD_DIR}
                    ctest --output-on-failure
                '''
            }
        }

        stage('Lint') {
            steps {
                sh '''
                    sudo apt-get update -y
                    sudo apt-get install -y clang-tidy
                    FILES=$(find . -name "*.c")
                    if [ -n "$FILES" ]; then
                        clang-tidy $FILES -- -I.
                    else
                        echo "No C files found to lint."
                    fi
                '''
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
