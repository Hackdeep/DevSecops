pipeline {
  agent any 
  tools {
    maven 'Maven'
  }
  stages {
    stage ('Initialize') {
      steps {
        sh '''
                    echo "PATH = ${PATH}"
                    echo "M2_HOME = ${M2_HOME}"
            ''' 
      }
    }
    
    stage ('Check-Git-Secrets') {
      steps {
        sh 'rm trufflehog || true'
        sh 'docker run gesellix/trufflehog --json https://github.com/Hackdeep/DevSecops.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    
    stage ('Source-Composition-Analysis') {
      steps {
        sh 'wget "https://raw.githubusercontent.com/Hackdeep/DevSecops/master/owasp-dependency-check.sh"'
        sh 'chmod +x owasp-dependency-check.sh'
        sh 'bash owasp-dependency-check.sh'
      }
    }
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StrictHostKeyChecking=no target/*.war ubuntu@54.210.81.90:/home/ubuntu/prod/apache-tomcat-9.0.69/webapps/webapp.war'
        }
      }
    } 
  }
}
  
    
    
