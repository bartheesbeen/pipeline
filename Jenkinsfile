pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'main']], // Let op de hoofdletter 'main'
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline.git']] // Vervang door jouw GitHub-repo URL
                    ])
                }
            }
        }

        stage('Delete HTML files on Server') {
            steps {
                // Execute shell commands to delete HTML files on the server.
                script {
                    def remoteServer = 'student@10.10.10.50'
                    sh "ssh ${remoteServer} 'cd /var/www/html && rm -f *.html'"
                }
            }
        }

        stage('Copy HTML files to Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server.
                script {
                    def remoteServer = 'student@10.10.10.50'
                    def localHTMLPath = "${env.WORKSPACE}/var/www/html" // Vervang door het juiste pad
                    sh "scp -r ${localHTMLPath} ${remoteServer}:/var/www/html"
                }
            }
        }
    }
}
