pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                // Checkout the Git repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkinstoken', url: 'https://github.com/RameezAhmedZaka/html-pipeline.git']])
            }
        }

        stage('Lint HTML') {
            environment {
                PATH = "/path/to/nodejs/bin:${env.PATH}"
            }
            steps {
                script {
                    echo 'Linting HTML...'
                    
                    // Install Node.js and npm if not already installed
                    sh 'curl -sL https://deb.nodesource.com/setup_14.x | sudo -E bash -'
                    sh 'sudo apt-get install -y nodejs'

                    // Install htmlhint
                    sh 'npm install -g htmlhint || true' // Install htmlhint, ignoring errors if already installed

                    // Run htmlhint
                    def result = sh(script: 'htmlhint index.html', returnStatus: true)
                    if (result != 0) {
                        error('HTML linting failed')
                    }
                }
            }
        }

        stage('Deploy to Apache') {
            steps {
                echo 'Deploying to Apache...'

                // Copy HTML files to Apache directory
                sh "cp -r * /var/www/html/"

                // Optionally, restart Apache to apply changes
                sh "sudo service apache2 restart"
            }
        }
    }
}
