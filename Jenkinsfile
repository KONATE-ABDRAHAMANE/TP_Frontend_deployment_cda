pipeline {
    agent { label 'agent-node' }
    stages {
        stage('Clonage du repo git') {
            steps {
                git branch: 'main', url: 'https://github.com/KONATE-ABDRAHAMANE/TP_Frontend_deployment_cda.git'
            }
        }
        stage('controle qualité'){
            steps {
                sh '''
                    sonar-scanner \
                    -Dsonar.projectKey=konate_tp_font \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=https://669b-212-114-26-208.ngrok-free.app \
                    -Dsonar.token=sqp_4edf723cf674fcab30066e048f9ea04b50cc2211
                '''
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