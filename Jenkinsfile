pipeline {
    agent any

    stages {
        stage('Checkout Code') {
            steps {
                script {
                    // Checkout code using Git in PowerShell
                    bat 'git clone https://github.com/bartheesbeen/pipeline.git'
                }
            }
        }

        stage('Deploy') {
            parallel {
                stage('Test Server') {
                    steps {
                        script {
                            // Use PowerShell to copy files to the test server
                            bat 'Copy-Item -Path ./* -Destination "student@10.10.10.50:/var/www/html/" -Recurse -Force'
                        }
                    }
                }
                stage('Production Server') {
                    when {
                        expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
                    }
                    steps {
                        script {
                            // Use PowerShell to copy files to the production server
                            bat 'Copy-Item -Path ./* -Destination "student@10.10.10.50:/var/www/html/" -Recurse -Force'
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
