pipeline {
    agent any

    // environment {
    //     GCLOUD_PROJECT = 'cloud-labs-405222'
    //     GCLOUD_ZONE = 'us-central1-a'
    //     GCLOUD_INSTANCE = 'jenkins-launched-server'
    //     GCLOUD_SERVICE_KEY = credentials('jenkins-sa.json')
    // }

    stages {
        stage('Checkout') {
            steps {
                // Checkout the code from the GitHub repository
                checkout scm
            }
        }

        stage('Build') {
            steps {
                // For a simple HTML site, no build step is needed
                // You might want to copy the files to a specific directory if needed
                //sh 'sudo apt -y install apache2'
                //sh 'wget https://archive.apache.org/dist/httpd/httpd-2.4.51.tar.gz'
                //sh 'tar -zxvf httpd-2.4.51.tar.gz'
                  echo 'Build and deployment started!'
            }
        }

        stage('Deploy') {
            steps {
                // Deploy the HTML site to a web server or hosting service
                // For example, copying the files to a web server directory
                sh 'cp -r * /var/www/html/'
                //sh 'mkdir -p $WORKSPACE/html/'
                //sh 'cp -r * $WORKSPACE/html/'
            }
        }
    }

    post {
        success {
            // Notification or additional steps on success
            echo 'Build and deployment successful!'
        }
        failure {
            // Notification or additional steps on failure
            echo 'Build or deployment failed!'
        }
    }
}
