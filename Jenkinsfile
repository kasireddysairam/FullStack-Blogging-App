
pipeline {
    agent any
    
tools {
        maven 'maven'  
    } 
    environment {
        SCANNER_HOME=tool 'sonarqube_scanner'
    }

    stages {
        stage('git checkout ') {
            steps {
                git branch: 'main', url: 'https://github.com/kasireddysairam/project_valaxy.git'
            }
        }

        stage('mvn compile') {
            steps {
                script {
                    // Run Maven or Gradle build
                    sh 'mvn  compile'
                }
            }
        }
        

        stage('Unit Test maven') {
            steps {
                script {
                    // Run Maven or Gradle build
                    sh 'mvn   test'
                }
            }
        }


         
        stage("Sonarqube Analysis "){
            steps{
                withSonarQubeEnv('sonarqube-server') {
                    sh ''' $SCANNER_HOME/bin/sonar-scanner -Dsonar.projectName=twitterapp \
                    -Dsonar.java.binaries=. \
                    -Dsonar.projectKey=twitterapp '''
    
                }
            }
        }
        
stage("SonarQube Quality Gate Check") {
            steps {
                script {
                def qualityGate = waitForQualityGate()
                    
                    if (qualityGate.status != 'OK') {
                        echo "${qualityGate.status}"
                        error "Quality Gate failed: ${qualityGateStatus}"
                    }
                    else {
                        echo "${qualityGate.status}"
                        echo "SonarQube Quality Gates Passed"
                    }
                }
            }
        }

stage('Maven Build : mave') {
            steps {
                script {
                    sh 'mvn clean install -DskipTests' 
                }
            }
        }

        

    }
}
