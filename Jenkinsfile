pipeline {
    parameters {
          string (name: 'DOCKER_REG',          defaultValue: 'hub.docker.com', description: 'Docker registry')
          string (name: 'DOCKER_TAG',          defaultValue: 'v1',                       description: 'Docker tag')
          string (name: 'DOCKER_REPO',         defaultValue: 'ci-cd-demo',          description: 'Docker repository')
          string (name: 'DOCKER_USR',          defaultValue: 'sachinkgaiwkad@gmail.com',              description: 'Docker repository user')
          string (name: 'DOCKER_PWD',          defaultValue: 'DOCKER_PWD',               description: 'Docker repository password')
    }


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
                archiveArtifacts artifacts: '**/target/*.war', fingerprint:true
                sh 'pwd'
                
            }
        }

        stage('Build  docker image') {
            agent {
                label 'master'
            }
            steps {
                echo 'Hello, Docker. Building  docker image...'
                unstash 'stash'
                sh 'docker build -t ${DOCKER_REG}/${DOCKER_REPO}:${BUILD_NUMBER} -f ${WORKSPACE}/Dockerfile .'
            }
        }
        stage('Push  docker image') {
            agent {
                label 'master'
            }
            steps {
                echo 'Pushing  docker image...'
                sh 'docker login -u ${DOCKER_USR} -p ${DOCKER_PWD} ${DOCKER_REG}'
                sh 'docker push ${DOCKER_REG}/${DOCKER_REPO}:${BUILD_NUMBER}'
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
