pipeline {
    agent any

    stages {
        stage('Checkout from GitHub') {
            steps {
                // Check out your code from GitHub.
                script {
                    def scmVars = checkout([
                        $class: 'GitSCM',
                        branches: [[name: 'Main']], // You can change the branch as needed
                        doGenerateSubmoduleConfigurations: false,
                        extensions: [
                            [$class: 'CloneOption', noTags: false, reference: '', shallow: false],
                            [$class: 'CleanBeforeCheckout'],
                        ],
                        userRemoteConfigs: [[url: 'https://github.com/bartheesbeen/pipeline.git']] // Replace with your GitHub repo URL
                    ])
                }
            }
        }

        stage('Delete HTML files on Server') {
            steps {
                // Execute shell commands to delete HTML files on the server.
                sh 'ssh student@ubuntu-server-2204 "cd /var/www/html && rm -f *.html"'
            }
        }

        stage('Copy HTML files to Server') {
            steps {
                // Copy HTML files from the checked-out repository to the server.
                sh 'scp -r https://github.com/bartheesbeen/pipeline.git*.html student@ubuntu-server-2204:/var/www/html'
            }
        }
    }
}
