stages {
        stage('Build1') { 
            steps { 
                sh 'docker ps' 
            }
        }
        stage('Test'){
            steps {
                sh 'docker ps -a' 
            }
        }
        stage('Deploy') {
            steps {
                sh 'docker --version'
            }
        }
    }
}