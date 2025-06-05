pipeline {
    agent { label 'agent-node' }
    stages {
        stage('Clonage du repo git') {
            steps {
                git branch: 'main', url: 'https://github.com/KONATE-ABDRAHAMANE/TP_Frontend_deployment_cda.git'
            }
        }
       stage('Déploiement du dépôt cloné via FTP') {
            steps {
                sh '''
                    lftp -d -u $siteUser,$sitePass ftp://ftp-lafia-market.alwaysdata.net -e "
                        mirror -R . www/;
                        bye
                    "
                '''
            }
        }
        stage('Installation du Node') {
            steps {
                sh """
                    sshpass -p "$sitePass" ssh -o StrictHostKeyChecking=no lafia-market@ssh-lafia-market.alwaysdata.net '
                        cd ~/www && npm install --no-dev
                      '
                """
            }
        }
        
    }
}