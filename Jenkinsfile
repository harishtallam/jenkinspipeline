pipeline {
    agent any

    parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.2.156', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.2.184', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
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

        stage ('Deployments'){
            parallel{
                stage ('Deploy to Staging'){
                    steps {
                        sh "scp -r **/target/*.war appuser@${params.tomcat_dev}:/home/appuser/apache-tomcat-8.5.37-staging/apache-tomcat-8.5.37/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp -r **/target/*.war appuser@${params.tomcat_prod}:/home/appuser/apache-tomcat-8.5.37-prod/apache-tomcat-8.5.37/webapps"
                    }
                }
            }
        }
    }
}