pipeline {
    agent any

    parameters {
        file(name: 'FILE', description: 'CSV file to be uploaded')
        choice(name: 'DOMAIN', choices: ['BEAUTY_DEFAULT', 'BEAUTY_NYKAA_D', 'FASHION'], description: 'Domain name')
        string(name: 'CHUNK_SIZE', defaultValue: '100', description: 'Chunk size')
    }

    stages {
        stage('Copy File to Remote Server') {
            steps {
                script {
                    def filePath = params.FILE
                    sh "scp ${filePath} ec2-user@172.26.17.194:/var/www/app/"
                }
            }
        }
        stage('Execute Commands on Remote Server') {
            steps {
                script {
                    def filePath = params.FILE
                    sh """
                    ssh ec2-user@172.26.17.194 <<EOF
                    echo "Domain: ${params.DOMAIN}"
                    echo "CHUNK_SIZE=${params.CHUNK_SIZE}"
                    cd /var/www/app/
                    sudo python3 manage.py activate_user_by_domain --domain ${params.DOMAIN} --user_id_csv_file ${filePath} --chunk_size ${params.CHUNK_SIZE}
                    EOF
                    """
                }
            }
        }
    }
}