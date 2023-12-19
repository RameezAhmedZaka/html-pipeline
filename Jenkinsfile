pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                // Checkout the Git repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkinstoken', url: 'https://github.com/RameezAhmedZaka/html-pipeline.git']])
            }
        }
    }
pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Install NodeJS') {
            steps {
                script {
                    // Install NodeJS using the configured tool
                    def nodejsHome = tool 'NodeJS'
                    env.PATH = "${nodejsHome}/bin:${env.PATH}"
                }
            }
        }

        stage('Lint HTML') {
            steps {
                script {
                    echo 'Linting HTML...'

                    // Install HTML linter (e.g., HTMLHint)
                    sh 'npm install -g htmlhint'

                    // Run HTML linter on the desired files
                    sh 'htmlhint /var/jenkins_home/workspace/HTML-pipeline*.html'
                }
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

