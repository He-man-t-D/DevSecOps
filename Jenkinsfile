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
        sh 'docker run gesellix/trufflehog --json https://github.com/He-man-t-D/DevSecOps.git > trufflehog'
        sh 'cat trufflehog'
      }
    }
    

    stage ('SAST') {
      steps {
        withSonarQubeEnv('sonar') {
          sh 'mvn sonar:sonar'
          sh 'cat target/sonar/report-task.txt'
        }
      }
    }
    
    stage ('Build') {
      steps {
      sh 'mvn clean package'
       }
    }
    
    stage ('Deploy-To-Tomcat') {
            steps {
                sh 'scp -o StrictHostKeyChecking=no target/*.war Hemant@74.225.211.113:prod/apache-tomcat-10.1.24/webapps/'
           }       
    }
        
stage('DAST'){
        steps{
                sh 'docker login'
              sh "docker run -t ghcr.io/zaproxy/zaproxy:stable zap-baseline.py -t http://74.225.211.113:8080/WebApp/ || true"  
        }
}
    
  }  
}
