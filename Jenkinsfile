pipeline {
    agent any
    environment {
        // Define static variables if needed
        AWS_REGION = 'us-east-2'
        DOMAIN_OWNER = '590184143844'
        DOMAIN_NAME = 'test-domain'
    }
    stages {
        stage('Get CodeArtifact Token') {
            steps {
                script {
                    // Use the 'sh' step to execute the command and capture the output
                    // The .trim() function removes any trailing newline characters
                    env.CODEARTIFACT_AUTH_TOKEN = sh(
                        script: "aws codeartifact get-authorization-token --domain ${env.DOMAIN_NAME} --domain-owner ${env.DOMAIN_OWNER} --region ${env.AWS_REGION} --query authorizationToken --output text",
                        returnStdout: true
                    ).trim()
                }
            }
        }
        stage('Use the Token') {
            steps {
                script {
                    // You can now access the token using env.CODEARTIFACT_AUTH_TOKEN
                    if (env.CODEARTIFACT_AUTH_TOKEN) {
                        sh 'echo "Successfully retrieved token!"'
                        // Example of using the token in another command
                        // sh "npm config set //${env.DOMAIN_NAME}-${env.DOMAIN_OWNER}.d.codeartifact.${env.AWS_REGION}.amazonaws.com/npm/my-repo/:_authToken=${env.CODEARTIFACT_AUTH_TOKEN}"
                    } else {
                        error "Failed to retrieve CodeArtifact token."
                    }
                }
            }
        }

        stage('Another Stage Using the Token') {
            steps {
                // The variable is available in subsequent stages as well
                echo "The token is also available here: ${env.CODEARTIFACT_AUTH_TOKEN}"
            }
        }
    }
}
