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
