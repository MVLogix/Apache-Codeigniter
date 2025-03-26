pipeline {
    agent any

    environment {
	SSH_CREDENTIALS_ID = 'remote_ssh_key'
    }

    
    stages {
        stage('Git clone') {
            steps {
                git credentialsId: 'GithubPAToken', url: 'https://github.com/MVLogix/Apache-Codeigniter.git', branch: 'main'
            }
        }
        
        
        stage('Deploy') {
            steps {
                script {

		    sshagent(credentials: [SSH_CREDENTIALS_ID]) {
                            

			// Set proper permissions for Apache to access the app
                    	sh 'sudo chown -R www-data:www-data /var/www/html/codeigniterapp'
                    	sh 'sudo chmod -R 755 /var/www/html/codeigniterapp'
                                        
                	sh 'cd /etc/apache2/sites-available'
        	        sh 'sudo a2ensite mycodeigapp.conf'
	                sh 'sudo systemctl reload apache2.service'

                    }
                    
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
