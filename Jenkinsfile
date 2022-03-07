pipeline {
    agent any 
    stages {
        stage('Stage 1') {
            steps {
                echo 'Hello world!' 
                sh "echo ${env.BUILD_ID}  ${env.BUILD_URL}"
            }
        }
    }
}