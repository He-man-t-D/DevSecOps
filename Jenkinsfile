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

    stage('check-secrets'){
      steps{
        sh 'rm trufflehog || true'
       sh 'docker run gesellix/trufflehog --json https://github.com/He-man-t-D/DevSecOps.git > trufflehog'
        sh 'cat trufflehog'
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
          sh 'scp -o StrictHostKeyChecking=no target/*.war Hemant@74.225.211.113:prod/apache-tomcat-10.1.24/webapps/WebApp.war'
          
        }
      }
    }
    
    
  }
}
