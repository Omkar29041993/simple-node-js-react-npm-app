pipeline {
    agent {
        docker {
            image 'node:6-alpine'
            args '-p 3000:3000'
        }
    }
    environment {
        CI = 'true'
        /* Override the npm cache directory to avoid: EACCES: permission denied, mkdir '/.npm' */
        'npm_config_cache=npm-cache',
        /* set home to our current directory because other bower
        * nonsense breaks with HOME=/, e.g.:
        * EACCES: permission denied, mkdir '/.config'
        */
        'HOME=.',
        
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
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
                input message: 'Finished using the web site? (Click "Proceed" to continue)'
                sh './jenkins/scripts/kill.sh'
            }
        }
    }
}
