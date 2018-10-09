pipeline {
    agent any

    triggers{
        pollSCM('* * * * *')
    }

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
        stage ('Deployments') {
            parallel {
                stage ('Deploy to Staging'){
                    steps {
                        sh "cp **/target/*.war /opt/tomcat-staging/webapps"
                    }
                }
                stage ('Deploy to Production'){
                    steps{
                        sh "cp **/target/*.war /opt/tomcat-prod/webapps"
                    }
                }
            }
        }
    }
}