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
                            // Gebruik plink om bestanden naar de testserver te kopiëren via SCP met SSH-wachtwoord
                            bat 'echo y | plink -ssh student@10.10.10.50 -pw student "cd /var/www/html/ && pscp -r *.* ."'
                        }
                    }
                }
                stage('Production Server') {
                    when {
                        expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
                    }
                    steps {
                        script {
                            // Gebruik plink om bestanden naar de productieserver te kopiëren via SCP met SSH-wachtwoord
                            bat 'echo y | plink -ssh jouw_ssh_gebruikersnaam@jouw_productie_server_ip -pw jouw_ssh_wachtwoord "cd /var/www/html/ && pscp -r *.* ."'
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
