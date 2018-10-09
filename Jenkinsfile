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
                        sh "curl -v -u tomcat:tomcat -T target/*.war http://172.30.3.31:8090/manager/text/deploy?path=&update=true"
                    }
                }
                stage ('Deploy to Production'){
                    steps{
                        sh "curl -v -u tomcat:tomcat -T target/*.war http://172.30.3.31:9090/manager/text/deploy?path=&update=true"
                    }
                }
            }
        }
    }
}