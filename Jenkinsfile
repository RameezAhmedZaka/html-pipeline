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
            steps {
                script {
                    echo 'Linting HTML...'

                    // Set up NVM environment
                    def nvmHome = tool name: 'NVM', type: 'hudson.plugins.nvm.NvmInstallation'
                    env.PATH = "${nvmHome}/bin:${env.PATH}"
                    sh 'export NVM_DIR="${nvmHome}" && [ -s "${NVM_DIR}/nvm.sh" ] && \\. "${NVM_DIR}/nvm.sh"'

                    // Install Node.js using NVM
                    sh 'nvm install 14 || true' // Install Node.js, ignoring errors if already installed

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
