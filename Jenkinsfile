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

                    // Install NodeJS using the configured tool
                    def nodejsHome = tool 'NodeJS'
                    env.PATH = "${nodejsHome}/bin:${env.PATH}"

                    // Install HTML linter (e.g., HTMLHint)
                    sh 'npm install -g htmlhint'

                    // Find HTML files in the workspace
                    def htmlFiles = findFiles(glob: '**/*.html')

                    // Run HTML linter on the found HTML files
                    sh "htmlhint ${htmlFiles.collect { "'${it}" }.join(' ')}"
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
