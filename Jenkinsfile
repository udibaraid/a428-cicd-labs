pipeline {
    agent {
        docker {
            image 'node:16-buster-slim'
            args '-p 3000:3000'
        }
    }
    stages {
        stage('Build') {
            steps {
                sh 'npm install'
            }
        }
        stage('Test') {
            steps {
                sh './jenkins/scripts/test.sh'
            }
        }

        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(
                        id: 'approvalInput',
                        message: 'Lanjutkan ke tahap Deploy?',
                        parameters: [choice(name: 'ACTION', choices: 'Proceed\nAbort', description: 'Pilih tindakan')],
                        submitter: 'user'
                    )

                    if (userInput == 'Proceed') {
                        echo "Pengguna memilih untuk melanjutkan."
                    } else {
                        error("Pengguna membatalkan eksekusi pipeline.")
                    }
                }
            }
        }
        
        stage('Deploy') { 
            steps {
                sh './jenkins/scripts/deliver.sh' 
                sh 'sleep 60' 
                sh './jenkins/scripts/kill.sh' 
            }
        }
    }
}