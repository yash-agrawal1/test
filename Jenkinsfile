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
        stage('Install PyYAML on Remote Server') {
            steps {
                sh '''
                ssh ec2-user@172.26.17.194 <<EOF
                if ! command -v pip &> /dev/null; then
                    curl https://bootstrap.pypa.io/get-pip.py -o get-pip.py
                    sudo python get-pip.py
                fi
                sudo pip install pyyaml
                EOF
                '''
            }
        }
        stage('Execute Commands on Remote Server') {
            steps {
                sh '''
                ssh ec2-user@172.26.17.194 <<EOF
                cd /home/ec2-user/user-service/app/
                DOMAIN=$(python -c "import yaml; print(yaml.safe_load(open('config.yaml'))['domain'])")
                CHUNK_SIZE=$(python -c "import yaml; print(yaml.safe_load(open('config.yaml'))['chunk_size'])")
                echo "Domain: $DOMAIN"
                echo "CHUNK_SIZE=$CHUNK_SIZE"
                sudo python manage.py activate_user_by_domain --domain $DOMAIN --user_id_csv_file sheet1.csv --chunk_size $CHUNK_SIZE
                '''
            }
        }
    }
}