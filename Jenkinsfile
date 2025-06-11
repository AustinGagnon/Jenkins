pipeline {
    agent any

    environment {
        // Define static variables
        AWS_REGION = 'us-east-2'
        DOMAIN_OWNER = '590184143844'
        DOMAIN_NAME = 'test-domain'
        REPO_NAME = 'POC_Repo'
    }

    

    stages {

        stage('Checkout Source Code') {
            steps {
                echo "Checking out code from repository..."
                git(
                    url: 'https://github.com/AustinGagnon/Jenkins.git', 
                    branch: 'main'                                      
                )
            }
        }

        stage('Init CodeArtifact Login') {
            steps {
                withAWS(credentials: 'my-aws-credentials', region: env.AWS_REGION) {
                    echo "AWS credentials configured. Fetching CodeArtifact token..."
                    sh "aws codeartifact login --tool npm --repository ${REPO_NAME} --domain test-domain --domain-owner ${env.DOMAIN_OWNER} --region ${env.AWS_REGION}"
                }
            }
        }

        stage('Build and Publish') {
            steps {
                // No 'dir()' block is needed since package.json is in the root
                timeout(time: 5, unit: 'MINUTES') {
                    echo "Running commands in workspace root: ${pwd()}"
                    
                    // Step 1: Install dependencies using the configured .npmrc
                    echo "Running npm install..."
                    sh 'npm cache clean --force'
                    sh 'npm install --verbose'

                    // Step 2: Set a unique package version using the build number
                    echo "Setting package version to 1.0.${env.BUILD_NUMBER}"
                    sh "npm version 1.0.${env.BUILD_NUMBER} --no-git-tag-version"

                    // Step 3: Publish the newly versioned package
                    echo "Publishing package..."
                    sh 'npm publish'
                }
            }
        }
    }
}


// MANUAL AUTH FOR CodeArtifact
// stage('Get CodeArtifact Token') {
    // steps {
    //     withAWS(credentials: 'my-aws-credentials', region: env.AWS_REGION) {
    //         script {
    //             echo "AWS credentials configured. Fetching CodeArtifact token..."
    //             // Now the aws command will be authenticated
    //             env.CODEARTIFACT_AUTH_TOKEN = sh(
    //                 script: "aws codeartifact get-authorization-token --domain ${env.DOMAIN_NAME} --domain-owner ${env.DOMAIN_OWNER} --query authorizationToken --output text",
    //                 returnStdout: true
    //             ).trim()
                
    //             // You can also unset the sensitive AWS keys for the rest of the stage if you want.
    //             // sh 'unset AWS_ACCESS_KEY_ID AWS_SECRET_ACCESS_KEY AWS_SESSION_TOKEN'
    //         }
    //     }
    // }
// }

// stage('Use the Token') {
//     steps {
//         script {
//             if (env.CODEARTIFACT_AUTH_TOKEN) {
//                 sh 'echo "Successfully retrieved token and it is available in this stage!"'
//                 // You can now use the token
//                 // Example:
//                 sh "echo Token: ${env.CODEARTIFACT_AUTH_TOKEN}"
//             } else {
//                 error "Failed to retrieve CodeArtifact token."
//             }
//         }
//     }
// }

// stage('Configure npm') {
//     steps {
//         script {
//             def registryUrl = "${env.DOMAIN_NAME}-${env.DOMAIN_OWNER}.d.codeartifact.${env.AWS_REGION}.amazonaws.com/npm/${env.REPO_NAME}/"
            
//             echo "Configuring .npmrc for registry: ${registryUrl}"

//             // Use the writeFile step to create the .npmrc file in the workspace root.
//             writeFile(
//                 file: '.npmrc', // Creates the file in the root of the workspace
//                 text: """
//                     //${registryUrl}:always-auth=true
//                     //${registryUrl}:_authToken=${env.CODEARTIFACT_AUTH_TOKEN}
//                 """
//             )
            
//             echo ".npmrc file created successfully."
//             sh 'cat .npmrc'
//         }
//     }
// }
