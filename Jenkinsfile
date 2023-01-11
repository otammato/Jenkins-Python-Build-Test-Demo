pipeline {
    agent any

    stages {
        stage('Hello') {
            steps {
                checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/otammato/jenkins-python-build-test-demo.git']])
            }
        }
        stage('Build') {
            steps {
                git branch: 'main', url: 'https://github.com/otammato/jenkins-python-build-test-demo.git'
                sh 'python3 ops.py'
            }
        }
        stage('Test') {
            steps {
                sh 'python3 -m pytest'
            }
        }
    }
}
