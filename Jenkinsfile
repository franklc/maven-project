pipeline {
    agent any
    environment {
        PATH = "/opt/apache-maven/bin:$PATH"
    }
    stages{
        stage('Initialize') {
            steps {
                sh '''
                    echo "Path = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
                '''
            }
        }
        stage('Build'){
            steps {
                sh 'mvn clean package'
            }
            post {
                success {
                    echo 'Now Archiving...'
                    archiveArtifacts artifacts: '**/target/*.war'
                }
            }
        }
    }
}