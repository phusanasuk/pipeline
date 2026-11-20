pipeline {
    agent any

    stages {
        stage('Parse Doxygen warnings') {
            steps {
                sh 'python3 parse_warnings.py doxygen-warnings.log -o warnings.csv'
                archiveArtifacts artifacts: 'warnings.csv'
            }
        }
    }
}
