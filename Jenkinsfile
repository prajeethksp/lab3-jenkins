pipeline {
    agent any

    stages {
        stage('Stage 1 -> Cloning Repo and moving files to bucket') {
            steps {
                script {
                    try {
                        // Your Stage 1 steps here
                        echo 'Cloning the Repo'
                        sh 'rm -fr *'
                        sh 'git clone https://github.com/prajeethksp/lab3-jenkins.git'
                        withCredentials([file(credentialsId: 'gcloud-creds', variable: 'GCLOUD_CREDS')]) {
        
                            sh 'gsutil -m cp -r /var/lib/jenkins/workspace/lab3/lab3-jenkins/* gs://test1lab'
                        }
                        sh "gcloud compute ssh  iamprajeeth@jenkins --zone=us-central1-a --command 'sudo apt install apache2 -y'"
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sh """
                        gcloud logging write Jenkins '{"Stage": "stage1", "Message":"Stage 1 execution Failed"}' --payload-type=json --severity=CRITICAL
                        """
                        error("Stage 1 failed: ${e.message}")
                    }
                }
            }
        }

        stage('Stage 2 -> Fetching files from bucket and deploying itm to production' ) {
            steps {
                script {
                    try {
                        sh 'rm -rf *'
                        //sh 'gsutil cp -r gs://test1lab/ /'
                        sh "gcloud compute ssh  iamprajeeth@jenkins --zone=us-central1-a --command 'sudo gsutil -m cp -r gs://test1lab/ /var/lib/jenkins/workspace/; cd lab3-jenkins; ls'"
                        sh "gcloud compute ssh  iamprajeeth@jenkins --zone=us-central1-a --command 'sudo rm -rf /var/www/html/* ;sudo mv /var/lib/jenkins/workspace/test1lab/* /var/www/html/'"
                    
                        //sh 'false'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sh """
                        gcloud logging write Jenkins '{"Stage": "stage2", "Message":"Stage 2 execution Failed"}' --payload-type=json --severity=CRITICAL
                        """
                        error("Stage 2 failed: ${e.message}")
                        
                    }
                }
            }
        }

        stage('Stage 3 -> Testing') {
            steps {
                script {
                    try {
                        sh 'curl http://35.202.6.167/'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sh """
                        gcloud logging write Jenkins '{"Stage": "stage3", "Message":"Stage 3 execution Failed"}' --payload-type=json --severity=CRITICAL
                        """
                        error("Stage 3 failed: ${e.message}")
                    }
                }
            }
        }
    }

    post {
        always {
            // This block will always run, regardless of the stage results
            echo "Pipeline completed. Result: ${currentBuild.result}"
        }
    }
}
