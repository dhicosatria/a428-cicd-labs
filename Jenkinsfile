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
                        id: 'Proceed1', message: 'Lanjutkan ke tahap Deploy?', parameters: [
                            [$class: 'BooleanParameterDefinition', defaultValue: true, description: 'Klik Proceed untuk melanjutkan atau Abort untuk menghentikan', name: 'Proceed']
                        ]
                    )
                    if (!userInput) {
                        error("Pipeline dihentikan oleh pengguna.")
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


