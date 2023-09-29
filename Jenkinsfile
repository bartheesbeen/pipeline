pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code using Git in cmd
                    bat 'git clone https://github.com/bartheesbeen/pipeline.git'
                }
            }
        }

        stage('Deploy') {
            parallel {
                stage('Test Server') {
                    steps {
                        script {
                            // Gebruik scp om bestanden naar de testserver te kopiëren met standaard SSH-client
                            bat 'echo y | scp -r -pw student ./* student@10.10.10.50:/var/www/html/'
                        }
                    }
                }
                stage('Production Server') {
                    when {
                        expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
                    }
                    steps {
                        script {
                            // Gebruik scp om bestanden naar de productieserver te kopiëren met standaard SSH-client
                            bat 'echo y | scp -r -pw jouw_ssh_wachtwoord ./* jouw_ssh_gebruikersnaam@jouw_productie_server_ip:/var/www/html/'
                        }
                    }
                }
            }
        }
    }

    post {
        always {
            cleanWs()
        }
    }
}
