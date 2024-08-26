pipeline {
    agent any

    environment {
        REACT_APP_VERSION = "1.0.$BUILD_ID"
        AWS_DEFAULT_REGION = "us-east-1"
    }


    stages {

        stage('deploy to AWS') {
            agent {
                docker {
                    image 'amazon/aws-cli'
                    reuseNode true
                    args "-u root --entrypoint=''"
                }
            }
            steps {
                withCredentials([usernamePassword(credentialsId: 'aws-cli', passwordVariable: 'AWS_SECRET_ACCESS_KEY', usernameVariable: 'AWS_ACCESS_KEY_ID')]) {
                    sh '''
                        aws --version
                        yum install jq -y
                        LATEST_TD_REVISION=$(aws ecs register-task-definition --cli-input-json file://aws/task-definition-prod.json | jq '.taskDefinition.revision')
                        echo $LATEST_TD_REVISION
                        aws ecs update-service --cluster LeasrnJenkinsApp-Cluster-Prod --service LearnJenkinsApp-Service-Prop --task-definition LearnJenkinsApp-TaskDefinition-Prod:$LATEST_TD_REVISION
                        aws ecs wait services-stable --cluster LeasrnJenkinsApp-Cluster-Prod --services LearnJenkinsApp-Service-Prop
                        echo '1'

                    '''
                }
            }
        }

        stage('Build') {
            agent {
                docker {
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps {
                sh '''
                echo 'Small'
                    ls -la
                    node --version
                    npm -version
                    npm ci
                    npm run build
                    ls -la
                '''
            }
        }



    }
}
