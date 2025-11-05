pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/chandrasekhar2004/jenkins-java-demo.git'
            }
        }

        stage('Build') {
            steps {
                echo 'Compiling Java sources...'
                // create classes folder and compile .java files
                sh 'mkdir -p target/classes'
                sh 'javac -d target/classes src/*.java'
                echo 'Build finished.'
            }
        }

        stage('Package') {
            steps {
                echo 'Packaging into JAR...'
                sh 'mkdir -p target'
                sh 'jar --create --file target/app.jar -C target/classes .'
                archiveArtifacts artifacts: 'target/app.jar', fingerprint: true
            }
        }

        stage('Deploy') {
            steps {
                echo 'Starting Deploy (simulated run of jar)...'
                // run the jar (in real you might copy to server, run scripts, etc.)
                sh 'java -jar target/app.jar'
            }
        }
    }
}
