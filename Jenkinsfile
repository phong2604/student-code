pipeline {
    agent { label 'student-code' }

    stages {

        stage('Build') {
            agent { docker { image 'ubuntu:22.04' } }
            steps {
                sh '''
                    apt-get update -y
                    apt-get install -y cmake build-essential
                    mkdir -p build
                    cd build
                    cmake ..
                    cmake --build .
                '''
            }
            post {
                success {
                    archiveArtifacts artifacts: 'build/**', fingerprint: true
                }
            }
        }

        stage('Test') {
            agent { docker { image 'ubuntu:22.04' } }
            steps {
                sh '''
                    apt-get update -y
                    apt-get install -y cmake
                    cd build
                    ctest --output-on-failure
                '''
            }
        }

        stage('Lint') {
            agent { docker { image 'ubuntu:22.04' } }
            steps {
                sh '''
                    apt-get update -y
                    apt-get install -y clang-tidy
                    FILES=$(find . -name "*.cpp" -or -name "*.c")
                    if [ -n "$FILES" ]; then
                        clang-tidy $FILES -- -I.
                    else
                        echo "No C/C++ files found to lint."
                    fi
                '''
            }
        }
    }
}
