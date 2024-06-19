pipeline {
    agent any
    tools {
  maven 'Maven-3.6'
        }
    stages {
        stage('git-checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/Bhuvaneshwar-PH/java-application.git'
            }
         }
        stage('mvn-test') {
            steps {
               sh 'mvn test'
            }
            }
        stage('mvn-deploy') {
            steps {
               sh 'mvn clean package'
            }
        }
        stage('docker-login') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'nexus-docker-repo', passwordVariable: 'PASSWD', usernameVariable: 'USERNAME')]) {
                     sh 'docker login -u $USERNAME -p $PASSWD 172.17.0.3:5000'
                }
            }
        }
        stage('docker-build') {
            steps {
               sh 'docker build -t 172.17.0.3:5000/mvn-pipeline_2:1.7 .'
            }
        }
        stage('docker-push') {
            steps {
               sh 'docker push 172.17.0.3:5000/mvn-pipeline_2:1.7'
            }
        }
        
    }   
    post {
      always {
           emailext (
               subject: " Pipeline Status: ${RESULT}",
               body: ''' <html>
                            <body>
                            <p>Build Status: ${}</p>
                            <p>Build Number: ${BUILD_NUMBER}</p>
                            <p>Check the <a href="${BUILD_URL}">console output</a></p>
                            </body>
                to: 'hbhuvaneshwar@gmail.com',
                from: 'jenkins@admin.com',
                replyTo:'jenkins@reply.com',
                mimeType: 'text/html' '''
              )       
        }
     }   
  }
