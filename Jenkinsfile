pipeline {
    agent any

    stages {
        stage('Stage 1: Cloning Repository and moving files into bucket') {
            steps {
                script {
                    try {
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
                        gcloud logging write Jenkins '{"Stage": "stage1", "Message":"Execution of Stage 1 Failed"}' --payload-type=json --severity=CRITICAL
                        """
                        error("Stage 1 failed: ${e.message}")
                    }
                }
            }
        }

        stage('Stage 2: Fetching files from bucket and deploying it') {
            steps {
                script {
                    try {
                        sh 'rm -rf *'
                        //sh 'gsutil cp -r gs://test1lab/ /'
                        //sh "gcloud compute ssh  iamprajeeth@jenkins --zone=us-central1-a --command 'sudo gsutil -m cp -r gs://test1lab/ /var/lib/jenkins/workspace/; cd lab3-jenkins; ls'"
                        //sh "gcloud compute ssh  iamprajeeth@jenkins --zone=us-central1-a --command 'sudo rm -rf /var/www/html/* ;sudo mv /var/lib/jenkins/workspace/test1lab/* /var/www/html/'"
                    
                        sh 'false'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sh """
                        gcloud logging write Jenkins '{"Stage": "stage2", "Message":"Execution of Stage 2 Failed"}' --payload-type=json --severity=CRITICAL
                        """
                        error("Stage 2 failed: ${e.message}")
                        
                    }
                }
            }
        }

        stage('Stage 3: Testing') {
            steps {
                script {
                    try {
                        sh 'curl http://35.202.6.167/'
                    } catch (Exception e) {
                        currentBuild.result = 'FAILURE'
                        sh """
                        gcloud logging write Jenkins '{"Stage": "stage3", "Message":"Execution of Stage 3 Failed"}' --payload-type=json --severity=CRITICAL
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
            echo "Pipeline finished. Result: ${currentBuild.result}"
        }
    }
}
