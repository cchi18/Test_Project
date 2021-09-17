pipeline {
    agent any
    tools {
        maven 'M2_HOME'
        jdk 'JAVA_HOME'
    }
    environment { 
        AWS_REGION = 'us-west-2'
        ECRREGISTRY = '735972722491.dkr.ecr.us-west-2.amazonaws.com' 
        IMAGENAME = 'haplet-cave'
        IMAGE_TAG = 'latest'
        ECS_CLUSTER = 'myapp-cluster'
        ECS_SERVICE = 'myapp-service'
    }
    stages {
       stage ('Cloning git & Build') {
          steps {
                checkout scm
            }
        }
        stage('Unit Tests Execution') {
            steps {
                sh 'mvn surefire:test'
            }
        }
        stage("Static Code analysis With SonarQube") {
            agent any
            steps {
              withSonarQubeEnv('SonarQube') {
                sh "mvn clean package sonar:sonar -Dsonar.host.url=http://54.85.48.113:9000 -Dsonar.login=8912b1866f72ab9c697e44d5befabf76bb4e16d0 -Dsonar.projectKey=jenkins -Dsonar.projectName=haplet-cave -Dsonar.projectVersion=1.0"
                sh "cp /var/lib/jenkins/workspace/Haplet-CI@2/webapp.war ."  
              }
            }
          } 
        stage('docker build and Tag Application') {
            steps { 
                 sh 'sudo docker build -t ${IMAGENAME} .'
                 sh 'sudo docker tag ${IMAGENAME}:${IMAGE_TAG} ${ECRREGISTRY}/${IMAGENAME}:${IMAGE_TAG}'
            }
        }
    }
}

         

