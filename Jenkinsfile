pipeline {
    agent any
    tools {
  maven 'mvn'
        }
    parameters {
        choice(choices: ['2.1', '2.2', '2.3'], description: 'Version of the application', name: 'Versions')
        booleanParam(defaultValue: true, description: 'Choose whether to run tests or not', name: 'ExecuteTest')
    }
    stages {
        stage('git-checkout') {
            steps {
               git branch: 'main', url: 'https://github.com/Bhuvaneshwar-PH/java-application.git'
            }
         }
        stage('mvn-test') {
            when {
                expression {
                    params.ExecuteTest == true
                }
            } 
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
                withCredentials([usernamePassword(credentialsId: 'nexus-cred', passwordVariable: 'passwd', usernameVariable: 'username')])  {
                    sh 'docker login -u $username -p $passwd 172.17.0.3:5000'
                }
            }
        }
        stage('docker-build') {
            steps {
               sh 'docker build -t 172.17.0.3:5000/mvn-pipeline_2:${params.Versions} .'
            }
        }
        stage('docker-push') {
            steps {
               sh 'docker push 172.17.0.3:5000/mvn-pipeline_2:${params.Versions}'
            }
        }
        
    }   
    post {
        always {
            script {
                    def jobName = env.JOB_NAME
                    def buildNumber = env.BUILD_NUMBER
                    def pipelineStatus = currentBuild.result ?: 'UNKNOWN'
                    def bannerColor = pipelineStatus.toUpperCase() == 'SUCCESS' ? 'green' : 
                    'red'
                    
                    def body = """<html>
                    <body>
                    <div style="border: 4px solid ${bannerColor}; padding: 
                    10px;">
                    <h2>${jobName} - Build ${buildNumber}</h2>
                    <div style="background-color: ${bannerColor}; padding: 
                    10px;">
                    <h3 style="color: white;">Pipeline Status: 
                    ${pipelineStatus.toUpperCase()}</h3>
                    </div>
                    <p>Check the <a href="${BUILD_URL}">console 
                    output</a>.</p>
                    </div>
                    </body>
                    </html>"""
                    emailext (
                    subject: "${jobName} - Build ${buildNumber} -${pipelineStatus.toUpperCase()}",
                    body: body,
                    to: 'hbhuvaneshwar@gmail.com',
                    from: 'jenkins@example.com',
                    replyTo: 'jenkins@example.com',
                    mimeType: 'text/html',
                    //attachmentsPattern: 'trivy-report.html'             
                        )
             }          
        } 
    } 
}
