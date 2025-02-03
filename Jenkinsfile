pipeline {
    agent any
    
    stages {
        stage('Clean Workspace') {
            steps {
                cleanWs()
            }
        }
        stage('Checkout Repository') {
            steps {
                git branch: 'main', url: 'https://github.com/yash-agrawal1/activate_test_user'
            }
        }
        stage('Copy Files to Remote Server') {
            steps {
                sh 'scp sheet1.csv ec2-user@172.26.17.194:/var/www/app/'
                sh 'scp config.yaml ec2-user@172.26.17.194:/var/www/app/'
            }
        }
        stage('Parse Config') {
            steps {
                script {
                    def output = sh(script: 'python3 yaml_parser.py', returnStdout: true).trim()
                    def lines = output.split('\n')
                    env.DOMAIN = lines[0].split(': ')[1]
                    env.CHUNK_SIZE = lines[1].split(': ')[1]
                }
            }
        }
        stage('Install on Remote Server') {
            steps {
                sh """
                ssh ec2-user@172.26.17.194 '
                hostname -i
                cd /var/www/app/
                pwd
                echo "Domain: ${env.DOMAIN}"
                echo "CHUNK_SIZE=${env.CHUNK_SIZE}"
                sudo python3 manage.py activate_user_by_domain --domain ${env.DOMAIN} --user_id_csv_file sheet1.csv --chunk_size ${env.CHUNK_SIZE}
                '
                """
            }
        }
    }
}