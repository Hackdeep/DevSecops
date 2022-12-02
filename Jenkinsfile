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
    stage ('Build') {
      steps {
      sh 'mvn clean package'
      }
    }
    stage ('Deploy-To-Tomcat') {
      steps {
        sshagent(['tomcat']) {
          sh 'scp -o StricktHostKeyChecking=no target/*.war ubuntu@54.210.81.90:/home/ubuntu/prod/apache-tomcat-9.0.69/webapps/webapp.war
        }
      }
    } 
  }
}
  
    
    
