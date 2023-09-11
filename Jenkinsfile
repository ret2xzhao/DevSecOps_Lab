pipeline {
    agent any
    tools {
        maven 'Maven'
    }
    stages {
        stage ('Initialize') {
            steps {
                script {
                    sh """
                        echo "PATH = ${PATH}"
                        echo "M2_HOME = ${M2_HOME}"
                        """
                }
            }
        }

        stage ('Build') {
            steps {
                script {
                    sh """
                        mvn clean package
                        """
                }
            }
        }

        stage ('Deploy-To-Tomcat') {
            steps {
              sshagent(['tomcat']) {
                  sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@3.133.148.2:/home/ubuntu/tomcat/apache-tomcat-9.0.79/webapps/webapp.war'
              }
            }
        }
    }
}
