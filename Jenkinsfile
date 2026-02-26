node {
    try {
        stage('Build') {
            sh '''
            echo "Building Java project..."
            cd "Password Protection"
            mkdir -p build
            # Compiling all java files in src
            javac -d build src/*.java
            echo "Build successful"
            '''
        }

        stage('Test') {
            sh '''
            echo "Running JUnit tests..."
            cd "Password Protection"
            
            # Use -s to check if file exists AND is not empty
            if [ ! -s junit-platform-console-standalone.jar ]; then
                echo "Downloading JUnit Standalone JAR..."
                curl -L -o junit-platform-console-standalone.jar \
                https://repo1.maven.org/maven2/org/junit/platform/junit-platform-console-standalone/1.10.0/junit-platform-console-standalone-1.10.0.jar
            fi

            mkdir -p test-build
            # Ensure test/*.java is on the same line as the javac command
            javac -cp "junit-platform-console-standalone.jar:build" -d test-build test/*.java
            
            java -jar junit-platform-console-standalone.jar \
            --class-path "build:test-build" \
            --scan-class-path
            '''
        }

        stage('Deploy') {
            sh '''
            echo "Packaging Application..."
            cd "Password Protection"
            jar cf FileEncrypter.jar -C build .
            echo "Deployment successful"
            '''
        }
    } catch (Exception e) {
        echo "Pipeline failed: ${e.message}"
        throw e
    }
}
