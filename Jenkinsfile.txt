pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                echo 'Source code is ready'
            }
        }

        stage('Build') {
            steps {
                bat '''
                    if exist out rmdir /s /q out
                    mkdir out
                    javac -d out Calculator.java
                '''
            }
        }

        stage('Run') {
            steps {
                bat '''
                    java -cp out Calculator
                    echo Build_OK > artifact.txt
                '''
            }
        }
    }

    post {
        always {
            archiveArtifacts artifacts: 'artifact.txt, out/**',
                             allowEmptyArchive: false
        }
    }
}