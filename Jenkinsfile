pipeline {
    agent any
    
        tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.126.23', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.222.200.82', description: 'Production Server')
    }

    triggers {
         pollSCM('* * * * *')
     }

stages{
        stage('Build'){
            steps {
                bat 'mvn clean package' 
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
                        bat "echo y|pscp -i "C:/Users/lhema/devOps/tomcat-demo.pem" "C:/Program Files (x86)/Jenkins/webapp.war" ec2-18-191-126-23.us-east-2.compute.amazonaws.com:/var/lib/tomcat8/webapps/webapp.war"
                    }
                }

                stage ("Deploy to Production"){
                    steps {
                        bat "scp -i /home/jenkins/tomcat-demo.pem **/target/*.war ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
