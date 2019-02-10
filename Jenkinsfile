pipeline {
    agent any
    
        tools {
        maven 'localMaven'
    }

    parameters {
         string(name: 'tomcat_dev', defaultValue: '18.191.246.30', description: 'Staging Server')
         string(name: 'tomcat_prod', defaultValue: '18.222.200.82', description: 'Production Server')
         string(name:'tomcat_stage',defaultValue:'C:\\Users\\lhema\\devOps\\tomcat-demo.ppk')
         string(name:'tomcat_stage1',defaultValue:'C:/Program Files (x86)/Jenkins/webapp.war')
         string(name:'war_file',defaultValue:'C:\\Program Files (x86)\\Jenkins\\workspace\\Fully-Automated\\webapp\\target\\webapp.war')
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
                         bat "echo y|pscp -i ${params.tomcat_stage} ${params.war_file} ec2-18.191.246.30.us-east-2.compute.amazonaws.com:/var/lib/tomcat8/webapps/webapp.war"
}
                }
                stage ("Deploy to Production"){
                    steps {
                       bat "echo y|pscp -i ${params.tomcat_stage} ${params.war_file} ec2-user@${params.tomcat_prod}:/var/lib/tomcat8/webapps"
                    }
                }
            }
        }
    }
}
