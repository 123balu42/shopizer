pipeline{
    agent any 
             parameters{
             choice(choices: 'chrome\nfirefox\nie' , description: 'choose browser name' , name: 'browser')
             choice(choices: 'false\ntrue'  , description: 'Not running on Selenium Grid?' , name: 'localRun')
        }

    stages{
        stage('Clone Sources'){
            steps{
                git url: 'https://github.com/123balu42/HappyTrip_Testing.git'
            }
        }
    //Build the project
     
        stage ('Build') {
            steps {
                    echo "Running job: ${env.JOB_NAME}\nbuild: ${env.BUILD_ID}"
                         bat '''
                                mvn -f HappyTrip/pom.xml clean install
                                '''
                        
            }
          post {
                success {
                    // we only worry about archiving the jar file if the build steps are successful
                    archiveArtifacts(artifacts: 'HappyTrip/reports/*.html', allowEmptyArchive: true)
                }
            }
        }
    }
    post {
        failure {
            mail to: '123balu42@gmail.com', from: '123balu42@gmail.com',
                subject: "Project Build: ${env.JOB_NAME} - Failed", 
                body: "Job Failed - \"${env.JOB_NAME}\" build: ${env.BUILD_NUMBER}"
        }
         success {
               emailext attachmentsPattern: '*reports/*.html', body: '''${SCRIPT, template="groovy-html.template"}''', 
                    subject: "${env.JOB_NAME} - Build # ${env.BUILD_NUMBER} - Successful", 
                    mimeType: 'text/html',to: "123balu42@gmail.com"
          } 
    }
}
