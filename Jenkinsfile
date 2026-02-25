pipeline {
    agent any

    stages {

        stage('Build') {
            steps {
                sh '''
                    echo "===== BUILD STAGE ====="
                    cd "Password Protection"
                    mkdir -p build
                    javac -d build src/*.java
                    echo "Build completed successfully"
                '''
            }
        }

        stage('Test') {
            steps {
                sh '''
                    echo "===== TEST STAGE ====="
                    cd "Password Protection"

                    # Download JUnit if not present
                    if [ ! -f junit-platform-console-standalone.jar ]; then
                        echo "Downloading JUnit..."
                        curl -L -o junit-platform-console-standalone.jar \
                        https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
                    fi

                    mkdir -p test-build

                    # Compile tests (if test folder exists)
                    if [ -d test ]; then
                        javac -cp junit-platform-console-standalone.jar:build -d test-build test/*.java
                        java -jar junit-platform-console-standalone.jar \
                            --class-path build:test-build \
                            --scan-class-path
                        echo "Tests executed"
                    else
                        echo "No test directory found - Skipping tests"
                    fi
                '''
            }
        }

        stage('Deploy') {
            steps {
                sh '''
                    echo "===== DEPLOY STAGE ====="
                    cd "Password Protection"
                    jar cf FileEncrypter.jar -C build .
                    echo "Artifact created: FileEncrypter.jar"
                '''
            }
        }
    }

    post {
        success {
            echo "Pipeline executed successfully!"
        }
        failure {
            echo "Pipeline failed!"
        }
    }
}
