pipeline {
    agent any

    stages {
        stage('Copy File to Remote Server') {
            steps {
                sh 'scp ${FILE} ec2-user@172.26.17.194:/var/www/app/'
            }
        }
        stage('Execute Commands on Remote Server') {
            steps {
                sh '''
                ssh ec2-user@172.26.17.194 <<EOF
                echo "Domain: $DOMAIN"
                echo "CHUNK_SIZE=$CHUNK_SIZE"
                cd /var/www/app/
                sudo python3 manage.py activate_user_by_domain --domain $DOMAIN --user_id_csv_file $FILE --chunk_size $CHUNK_SIZE
                EOF
                '''
            }
        }
    }
}