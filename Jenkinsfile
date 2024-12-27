pipeline {
    agent {
        node{
            label "java-slave"
        }
    }
   environment {
       SONAR_HOST_URL= "http://13.201.40.229:9000"
        SONAR_AUTH_TOKEN = credentials('sonar-token')
        PATH = "/opt/apache-maven-3.9.9/bin:$PATH"
   }
    
    stages {
        
         stage('cleanup') {
            steps {
                // Clean workspace directory for the current build
            
                deleteDir ()   
            }
        }
        stage('git checkout') {
            steps {

                git branch: 'main', url: 'https://github.com/kasireddysairam/project_valaxy.git'
            }
        }
     stage(' mvn build') {
            steps {

        sh 'mvn clean  install  -Dmaven.test.skip=true'
            }
        }

        stage('Unit Test') {
            steps {
               
                sh 'mvn surefire-report:report'
            }
        }
stage('SonarQube Analysis') {
            steps {
                withCredentials([string(credentialsId: 'sonar-token', variable: 'SONAR_TOKEN')]) {
                    sh '''
                        sonar-scanner \
                            -Dsonar.projectKey=${JOB_NAME} \
                            -Dsonar.projectName=${JOB_NAME} \
                           
                            -Dsonar.login=${SONAR_TOKEN}
                    '''
                }
            }
        }



        
    }

}
