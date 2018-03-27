pipeline {
    agent any

    tools {
        maven 'localmvn'
    }
 
    parameters {
         string(name: 'tomcat_dev', defaultValue: '192.168.0.107', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '192.168.0.108', description: 'Production Server')
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
                        sh "scp **/target/*.war ${params.tomcat_dev}:/docs/apache-tomcat-8.5.29-staging/webapps"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        sh "scp **/target/*.war ${params.tomcat_prod}:/docs/apache-tomcat-8.5.29-prod/webapps"
                    }
                }
            }
        }
    }
}