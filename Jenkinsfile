pipeline {
    agent any
    stages {
        stage('CI') {
            steps {
                snDevOpsStep()
                sh 'mvn compile'
                sh 'mvn verify'
                //withSonarQubeEnv('SonarQube') {
                //sh "mvn clean package sonar:sonar"
              }
            }
            post {
                success {
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
        }
    
     stage('UAT deploy') {
            steps {
                snDevOpsStep()
               
                sh 'mvn package'
                sh 'cp -vfr target/globex-web.war /var/www/html/UAT/globex-uat.war'
                sh 'sudo systemctl stop httpd'
                sh 'echo ################ Reiniciando o HTTPD #############'
                sh 'sudo systemctl start httpd'
            
            }
        }

    
     stage('UAT Homologacao') {
            steps {
                snDevOpsStep()
               
                sh 'mvn verify'
                sh 'cp -vfr target/globex-web.war /var/www/html/UAT/globex-uat.war'
                sh 'sudo systemctl stop httpd'
                sh 'echo ################ Reiniciando o HTTPD #############'
                sh 'sudo systemctl start httpd'
            
            }
          post {
                success {
                    junit '**/target/surefire-reports/*.xml' 
                }
            }
        }
    
    stage('Producao') {
            steps {
                snDevOpsStep()
                snDevOpsChange()
                sh 'cp -vfr target/globex-web.war /var/www/html/UAT/globex-prod.war'
                sh 'sudo systemctl stop httpd'
                sh 'echo ################ Reiniciando o HTTPD #############'
                sh 'sudo systemctl start httpd'
            
            }
        }
    }
}
    
