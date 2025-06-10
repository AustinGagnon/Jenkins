pipeline {
    agent any

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                pwd
                sh """
                  npm init
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