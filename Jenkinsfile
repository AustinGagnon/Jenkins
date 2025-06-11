pipeline {
    agent any

    environment {
        // Define static variables
        AWS_REGION = 'us-east-2'
        DOMAIN_OWNER = '590184143844'
        DOMAIN_NAME = 'test-domain'
    }

    stages {
        stage('Get CodeArtifact Token') {
            // This wrapper injects the AWS credentials as environment variables
            // for all the steps within this block.
            // Replace 'my-aws-credentials' with the ID you created in Jenkins.
            withAWS(credentials: 'my-aws-credentials', region: env.AWS_REGION) {
                steps {
                    script {
                        echo "AWS credentials configured. Fetching CodeArtifact token..."
                        // Now the aws command will be authenticated
                        env.CODEARTIFACT_AUTH_TOKEN = sh(
                            script: "aws codeartifact get-authorization-token --domain ${env.DOMAIN_NAME} --domain-owner ${env.DOMAIN_OWNER} --query authorizationToken --output text",
                            returnStdout: true
                        ).trim()
                        
                        // You can also unset the sensitive AWS keys for the rest of the stage if you want.
                        // sh 'unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN'
                    }
                }
            }
        }

        stage('Use the Token') {
            steps {
                script {
                    if (env.CODEARTIFACT_AUTH_TOKEN) {
                        sh 'echo "Successfully retrieved token and it is available in this stage!"'
                        // You can now use the token
                        // Example:
                        sh "echo Token: ${env.CODEARTIFACT_AUTH_TOKEN}"
                    } else {
                        error "Failed to retrieve CodeArtifact token."
                    }
                }
            }
        }
    }
}
