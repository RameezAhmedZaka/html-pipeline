pipeline {
    agent any

    stages {
        stage('check') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkinstoken', url: 'https://github.com/RameezAhmedZaka/html-pipeline.git']])
            }
        }

        stage('Lint HTML') {
            steps {
                script {
                    echo 'Linting HTML...'
                    sh 'npm install -g htmlhint || true' // Install htmlhint, ignoring errors if already installed
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
