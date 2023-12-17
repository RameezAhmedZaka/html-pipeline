pipeline {
    agent any

    stages {
        stage('Lint HTML') {
            steps {
                // Checkout code from GitHub
                checkout scm
                
                // Run HTML linter (using htmlhint as an example)
                sh 'npm install -g htmlhint'
                sh 'htmlhint index.html'
            }
        }

        stage('Deploy to Apache Server') {
            when {
                // Run this stage only if the previous stage succeeds
                expression { currentBuild.resultIsBetterOrEqualTo('SUCCESS') }
            }
            steps {
                // Copy HTML files to Apache server directory
                sh 'cp index.html /path/to/apache/www/'

                // Restart Apache server if necessary
                sh 'systemctl restart apache2'
            }
        }
    }

    post {
        // Notify on failure
        failure {
            emailext subject: 'Pipeline Failed',
                      body: 'The Jenkins pipeline has failed. Please check the build logs for details.',
                      to: 'rameezahmad145@gmail.com'
        }
    }
}
