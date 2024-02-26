pipeline {
    agent { label 'linux' }


   environment {
        TOMCAT_HOME = '/opt/tomcat'
        WAR_FILE = '*.war'
        REMOTE_HOST = '20.84.52.241'
        REMOTE_USER = 'azureuser'
    }


   stages {

       stage('Build') {
            steps {
                sh 'mvn clean package'
            }
        }

      stage('BKP') {
            steps {
                script {
                    

                        sh '''
                        ssh azureuser@20.84.52.241 /opt/tomcat/bin/shutdown.sh
                        ssh azureuser@20.84.52.241 DATE=$(date +%Y-%m-%d-%s) 
                        ssh azureuser@20.84.52.241 mv /opt/tomcat/webapps/*.war /opt/tomcat/webapps/${DATE}.war || true
                        '''

                    
                }
            }
        }

       stage('Deploy') {
            steps {
                script {
                        
                        sh "mv /home/azureuser/dev/dev/workspace/matrimony_pipeline_demo/target/*.war /home/azureuser/dev/dev/workspace/matrimony_pipeline_demo/target/matrimony.war"

                        sh "scp /home/azureuser/dev/dev/workspace/matrimony_pipeline_demo/target/*.war azureuser@20.84.52.241:/opt/tomcat/webapps && ssh azureuser@20.84.52.241 /opt/tomcat/bin/startup.sh"
                    
                }
            }
        }
    }
}
