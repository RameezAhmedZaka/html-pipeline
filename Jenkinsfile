pipeline {
    agent any

    stages {
        stage('Checkout') {
            steps {
               checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'jenkins', url: 'https://github.com/RameezAhmedZaka/html-pipeline.git']])
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
                        sh 'apt-get update'
                        sh 'curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.35.3/install.sh | bash'
                        sh 'apt-get update'
                        sh 'apt-get install -y npm'
                        sh 'npm install -g htmlhint'
                        sh 'cd /var/jenkins_home/workspace/'
                        sh 'htmlhint ./'
                        echo 'HTML Linter passed'
                    }
                    catch (e) {
                        echo 'HTML Linter failed'
                        currentBuild.result = 'FAILURE'
                        throw e
                
                    }
                }
            }
        }

        stage('Deploy Code') {
            
            steps {
                echo 'Deploying Code to Apache Server'

                script {
                    sh ' apt-get remove --purge apache2 apache2-utils'
                    sh 'apt install -y apache2'
                    sh 'cp -r * /var/www/html/'
                    sh 'rm -rf *'
                    sh 'cd /var/www/'
                    sh 'service apache2 restart'
        
                }
            }
        }
    }
    
}
