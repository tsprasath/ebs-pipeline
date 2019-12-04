pipeline {
    agent any
 
    options {
        timestamps()
        timeout(time: 1, unit: 'HOURS')
        buildDiscarder(logRotator(numToKeepStr: '10'))
        disableConcurrentBuilds()
    }

stages {

           stage("Prepare environment and Deploy") {
           agent {
                 docker {
                     image 'chriscamicas/awscli-awsebcli'
                     args '-u root:root'
                     reuseNode true
                 }
             }
            steps {
                //Deploy to AWS using Security credentials: ACCESS_KEY and SECRET from custom IAM Jenkins user
                    withCredentials([[$class: 'UsernamePasswordMultiBinding', credentialsId: 'AWS', usernameVariable: 'AWS_ACCESS_KEY_ID', passwordVariable: 'AWS_SECRET_ACCESS_KEY']]) {

                sh '''
                eb init ebs -p "64bit Amazon Linux 2017.09 v2.6.4 running Java 8" --region "ca-central-1"
                aws elasticbeanstalk create-application-version --application-name my-app --version-label v1 --description myapp --source-bundle S3Bucket="myapp2126",S3Key="aws-s3-ebsdeploy.zip" --auto-create-application
                aws elasticbeanstalk create-environment --application-name my-app --environment-name my-env --version-label v1 --solution-stack-name "64bit Amazon Linux 2015.03 v1.4.6 running PHP 5.6"
                '''
            }
            }
           }
post {
        always {
            echo 'One way or another, I have finished'
            cleanWs() /* clean up our workspace */
        }
        success {
           echo 'I succeeded!'
        }
        unstable {
            echo 'I am unstable :/'
        }
        
        failure {
            echo 'I failed :('
        }
        
        changed {
            echo 'Things were different before...'
        } 
    }  
}//pipeline ends


          
