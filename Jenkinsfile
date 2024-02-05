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
        stage('Deploy') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('Manual Approval') {
            steps {
                script {
                    def userInput = input(message: 'Lanjutkan ke tahap Deploy?', ok: 'Proceed', parameters: [choice(choices: 'Proceed\nAbort', description: 'Pilih Proceed untuk melanjutkan atau Abort untuk menghentikan', name: 'ACTION')])
                    if (userInput == 'Proceed') {
                        echo 'Melanjutkan ke tahap Deploy...'
                    } else {
                        error('Pipeline dihentikan oleh pengguna.')
                    }
                }
            }
        }
        stage('Deploy') {
            steps {
               sh './jenkins/scripts/deliver.sh' 
                input message: 'Sudah selesai menggunakan React App? (Klik "Proceed" untuk mengakhiri)' 
                sh './jenkins/scripts/kill.sh'
                sh 'sleep 60' 
            }
        }
    }
}
