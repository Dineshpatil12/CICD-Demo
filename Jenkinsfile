pipeline {
    agent any 
    tools {
      maven 'maven3
    }
    stages {
        stage('Build and Test') {
            agent { node{
                       label "jenkins"}
            } 
            steps {
                sh 'mvn clean package'
                sh 'echo "build ran"'
                archiveArtifacts artifacts: './target/root.war', fingerprint:true
              
            }
        }
        
        }
        
        stage ('Deploy to Integration') {
            agent {node{
                   label "jenkins"}
            }
            steps {
                build job:'../Tomcat deploy to Integration' , parameters:[string(name: 'BRANCH_NAME', value: "${env.BRANCH_NAME}")]
            }
        }
    }
}

