pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                // Checkout the Git repository
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkinstoken', url: 'https://github.com/RameezAhmedZaka/html-pipeline.git']])
            }
        }

        stage('HTML Linter') {
            steps {
                echo 'Running HTML Linter'

                script {
                    try {
                        sh 'rm -rf /var/jenkins_home/workspace/HTML-pipeline@temp/'
                        sh 'rm -rf /var/jenkins_home/workspace/HTML-pipeline@2/'
                        sh 'rm -rf /var/jenkins_home/workspace/HTML-pipeline@2@temp/'   
                        sh 'export PATH=~/.npm-global/bin:$PATH'
                        sh 'source ~/.bashrc'
                        sh 'npm install -g htmlhint'
                        sh 'cd /var/jenkins_home/workspace/'
                        sh 'htmlhint ./'
                        echo 'HTML Linter passed'
                    }
                    catch (Exception e) {
                        echo 'HTML Linter failed'
                        currentBuild.result = 'FAILURE'
                    }
                }
            }
        }

        stage('Deploy Code') {
            when {
                expression { currentBuild.result == 'SUCCESS' }
            }

            steps {
                echo 'Deploying Code to Apache Server'

                script {
                    // Copy HTML files to Apache directory
                    sh "cp -r * /var/www/html/"

                    // Optionally, restart Apache to apply changes
                    sh "sudo service apache2 restart"
                }
            }
        }
    }
    
}
