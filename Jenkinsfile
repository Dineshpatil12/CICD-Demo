pipeline {
    agent any 
    tools {
      maven 'maven3'
    }
    stages {
        stage('Build and Test') {
            agent { node{
                       label "jenkins"}
            } 
            steps {
                sh 'mvn clean package'
                sh 'echo "build ran"'
                archiveArtifacts artifacts: 'demo_master-6YYR567VJJJPTGPUEQMMGURWFRTMNWXOQR4VB2WO4LICI6G4LPTQ@2/target/root.war', fingerprint:true
            }
        }
        
        stage ('Deploy to Integration') {
            agent {node{
                   label "jenkins"}
            }
            steps {
                build job:'../tomcat deploy' , parameters:[string(name: 'BRANCH_NAME', value: "${env.BRANCH_NAME}")]
            }
        }
    }
}
