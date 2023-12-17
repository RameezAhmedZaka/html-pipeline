pipeline {
    agent any

    stages {
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
    }

    stage('Deploy to Apache') {
    steps {
        echo 'Deploying to Apache...'
        def apacheDir = '/var/www/html'

        sh "mkdir -p ${apacheDir}"

        // Copy HTML files to Apache directory
        sh "cp -r * ${apacheDir}/"

        // Optionally, restart Apache to apply changes
        sh "sudo service apache2 restart"
    }
}

}

