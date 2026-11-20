pipeline {
    agent any

    parameters {
        booleanParam(name: 'USE_SAMPLE_LOG', defaultValue: false, description: 'Test the parser with a sample warning instead of a real Doxygen log.')
    }

    stages {
        stage('Checkout RepoC') {
            steps {
                dir('RepoC') {
                    git branch: 'main', url: 'https://github.com/phusanasuk/RepoC.git'
                }
            }
        }

        stage('Parse Doxygen warnings') {
            steps {
                script {
                    if (params.USE_SAMPLE_LOG) {
                        writeFile file: 'sample-doxygen-warnings.log', text: 'src/main.cpp:42: warning: Missing documentation\n'
                        sh 'python3 RepoC/parse_warnings.py sample-doxygen-warnings.log -o sample-warnings.csv'
                        archiveArtifacts artifacts: 'sample-warnings.csv'
                    } else {
                        if (!fileExists('doxygen-warnings.log')) {
                            error 'Missing doxygen-warnings.log. Generate it in the workspace before parsing, or select USE_SAMPLE_LOG to test the parser.'
                        }
                        sh 'python3 RepoC/parse_warnings.py doxygen-warnings.log -o warnings.csv'
                        archiveArtifacts artifacts: 'warnings.csv'
                    }
                }
            }
        }
    }
}
