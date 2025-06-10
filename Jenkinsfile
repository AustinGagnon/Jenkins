pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh " pwd "
                sh " whoami "
                sh """
                  npm init
                  ls -la
                """

            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
            }
        }
        stage('Deploy') {
            steps {
                echo 'Deploying....'
            }
        }
    }
}
