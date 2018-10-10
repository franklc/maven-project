pipeline {
    agent any

    tools {
        maven 'localMaven'
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
        stage('Build app'){
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
        stage ('Build app container'){
            steps {
                sh "docker build . -t tomcatwebapp:${env.BUILD_ID}"
            }
        }
    }
}