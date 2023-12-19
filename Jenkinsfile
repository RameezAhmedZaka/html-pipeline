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

                 
                    sh 'mkdir ~/.npm-global'
                    sh 'export PATH=~/.npm-global/bin:$PATH'
                    sh 'source ~/.bashrc'
                    sh 'npm install -g htmlhint'

                    // Run HTML linter on the desired files
                    sh 'htmlhint /var/jenkins_home/workspace/HTML-pipeline*.html'
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

