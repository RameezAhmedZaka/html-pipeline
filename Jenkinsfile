pipeline {
    agent any

    stages {
        stage('Hello') {
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
                    catch (Exception e) {
                        echo 'HTML Linter failed'
                        currentBuild.result = 'FAILURE'
                        
                    }
                }
            }
        }

        stage('Deploy Code') {
        
            steps {
                echo 'Deploying Code to Apache Server'

                script {
                    sh 'apt install -y apache2'
                    sh 'cp * /var/www/html/'
                    sh 'apt-get update'
                    sh 'apt-get install -y nano'
     

        sh '''

         sed -i 's/AllowOverride None/AllowOverride All/g' /etc/apache2/apache2.conf

        '''
                    sh 'cd /var/www/html/'
                    sh 'rm Jenkinsfile'
                    // Optionally, restart Apache to apply changes
                    sh 'service apache2 restart'
                  
                }
            }
        }
    }
    
}
