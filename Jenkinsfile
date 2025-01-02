pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Hello Building'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Hello Deploying'
                echo 'Deploying'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'MyAWS',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://test-env-jenkins/')
                }
            }
        }
        stage('Test') {
            steps {
                echo 'Hello Testing'
                script {
                    def url = 'https://test-env-jenkins.s3.ap-northeast-1.amazonaws.com/index.html'
                    def response = sh(script: "curl -s -o /dev/null -w '%{http_code}' '$url'", returnStdout: true)

                    if (response == '200') {
                        echo 'Test OK'
                    } 
                    // else {
                    //     echo response
                    //     error 'Test NG'
                    // }
                }
            }
        }
        stage('Release') {
            steps {
                echo 'Hello Releasing'
                withCredentials([[
                    $class: 'AmazonWebServicesCredentialsBinding',
                    credentialsId: 'MyAWS',
                    accessKeyVariable: 'AWS_ACCESS_KEY_ID',
                    secretKeyVariable: 'AWS_SECRET_ACCESS_KEY']]){
                        sh(script: 'aws s3 cp /var/lib/jenkins/workspace/JenkinsPipeline/index.html s3://prd-env-jenkins/')
                }
            }
        }
    }
}
