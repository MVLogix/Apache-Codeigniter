pipeline {
    agent any

    
    stages {
        stage('Git clone') {
            steps {
                git credentialsId: 'GithubPAToken', url: 'https://github.com/MVLogix/Apache-Codeigniter.git', branch: 'main'
            }
        }
        
        stage('Install Dependencies') {
            steps {
                script {
                    sh 'apt install apache2 php php-cli php-common php-imap php-redis php-snmp php-xml php-zip php-mbstring php-curl libapache2-mod-php php-intl -y'
					
					sh 'curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer'
					
					sh 'a2enmod rewrite'
					
                    sh 'systemctl restart apache2'
                }
            }
        }
        
        
        stage('Deploy Locally') {
            steps {
                script {
                    
                    // Set proper permissions for Apache to access the app
                    sh 'chown -R www-data:www-data /var/www/html/codeigniterapp'
                    sh 'chmod -R 755 /var/www/html/codeigniterapp'
					
		    sh 'cd /etc/apache2/sites-available'
		    sh 'a2ensite mycodeigapp.conf'
		    sh 'systemctl reload apache2.service'
                }
            }
        }
    }
    
    post {
        always {
            // Clean up steps
            echo 'Cleaning up...'
        }
        success {
            // Notify success
            echo 'The pipeline ran successfully!'
        }
        failure {
            // Notify failure
            echo 'There was an error in the pipeline.'
        }
    }
}
