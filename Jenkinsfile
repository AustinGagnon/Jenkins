pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh " pwd "
                sh " whoami "
                sh """
                  git --version
                  // npm init
                  ls -la
                  export CODEARTIFACT_AUTH_TOKEN=`aws codeartifact get-authorization-token --domain test-domain --domain-owner 590184143844 --region us-east-2 --query authorizationToken --output text`
                  echo $CODEARTIFACT_AUTH_TOKEN
                """

            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                echo '$CODEARTIFACT_AUTH_TOKEN'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
