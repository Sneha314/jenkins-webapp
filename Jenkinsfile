
pipeline {
    agent any

    options {
        skipDefaultCheckout(true)
        timestamps()
    }

    stages {

        stage('Checkout') {
            steps {
                echo 'Downloading source code from GitHub'

                checkout scm

                sh 'ls -la'
            }
        }

        stage('Deploy') {
            steps {
                echo 'Deploying application to Apache'

                sh '''
                    rsync -av --delete \
                    --exclude='.git/' \
                    --exclude='Jenkinsfile' \
                    "$WORKSPACE/" /var/www/html/
                '''

            }
        }

        stage('Verify') {
            steps {
                echo 'Checking website deployment'

                sh '''
                    test -f /var/www/html/index.html
                    curl -fsS http://localhost/ -o /dev/null
                    echo "Website deployment verified successfully"
                '''
            }
        }
    }
}

