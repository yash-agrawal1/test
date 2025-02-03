pipeline {
    agent any

    parameters {
        choice(name: 'DOMAIN', choices: ['BEAUTY_DEFAULT', 'BEAUTY_NYKAA_D', 'FASHION'], description: 'Domain name')
        string(name: 'CHUNK_SIZE', defaultValue: '100', description: 'Chunk size')
    }

    stages {
        stage('Checkout Repository') {
            steps {
                git url: 'https://github.com/yash-agrawal1/activate_test_user'
            }
        }
        stage('Copy File to Remote Server') {
            steps {
                sh 'scp sheet1.csv ec2-user@172.26.17.194:/var/www/app/'
            }
        }
        stage('Execute Commands on Remote Server') {
            steps {
                sh '''
                ssh ec2-user@172.26.17.194 <<EOF
                echo "Domain: ${DOMAIN}"
                echo "CHUNK_SIZE=${CHUNK_SIZE}"
                cd /var/www/app/
                sudo python3 manage.py activate_user_by_domain --domain ${DOMAIN} --user_id_csv_file sheet1.csv --chunk_size ${CHUNK_SIZE}
                EOF
                '''
            }
        }
    }
}