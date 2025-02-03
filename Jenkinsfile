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
        stage('Install on Remote Server') {
            steps {
                sh """
                ssh ec2-user@172.26.17.194 'hostname -i' 
                ssh ec2-user@172.26.17.194 'cd /var/www/app/'
                ssh ec2-user@172.26.17.194 'DOMAIN=\$(python3 -c "import yaml; print(yaml.safe_load(open('config.yaml'))['domain'])")' 
                ssh ec2-user@172.26.17.194 'CHUNK_SIZE=\$(python3 -c "import yaml; print(yaml.safe_load(open('config.yaml'))['chunk_size'])")'
                ssh ec2-user@172.26.17.194 'echo "Domain: \$DOMAIN"'
                ssh ec2-user@172.26.17.194 'echo "CHUNK_SIZE=\$CHUNK_SIZE"'
                ssh ec2-user@172.26.17.194 'sudo python3 manage.py activate_user_by_domain --domain \$DOMAIN --user_id_csv_file sheet1.csv --chunk_size \$CHUNK_SIZE'

                """
            
                
                '''
            }
        }
    }
}