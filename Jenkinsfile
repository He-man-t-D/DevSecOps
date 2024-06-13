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

    stage('Deploy-to-tomcat'){
      steps{
        sshagent(['tomcat']){
          sh 'scp -o StrictHostKeyChecking=no target/*.war Hemant@74.225.211.113:prod/apache-tomcat-10.1.24/webapps/'
          
        }
      }
    }
    
    
  }
}
