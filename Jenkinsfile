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

        stage ('Check-Git-Secrets') {
            steps {
              sh 'rm trufflehog || true'
              sh 'docker run gesellix/trufflehog --json https://github.com/ret2xzhao/DevSecOps_Lab.git > trufflehog'
              sh 'cat trufflehog'
            }
        }

        stage ('Source-Composition-Analysis') {
            steps {
              sh 'rm owasp* || true'
              sh 'wget "https://raw.githubusercontent.com/ret2xzhao/DevSecOps_Lab/master/owasp-dependency-check.sh" '
              sh 'chmod +x owasp-dependency-check.sh'
              sh 'bash owasp-dependency-check.sh'
              sh 'cat /var/lib/jenkins/OWASP-Dependency-Check/reports/dependency-check-report.xml'
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
