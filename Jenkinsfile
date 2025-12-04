pipeline {
    agent any

    stages {
        stage('Create web directory') {
            input {
                message 'Enter the data'
                parameters {
                    string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment')
                    string(name:'ENVIRONMENT', defaultValue: 'Development', description: 'Environment to deploy')
                }
            }
            steps {
                echo "The responsible of this project is ${AUTHOR} and will be deployed in ${ENVIRONMENT}"
                // First, drop the directory if it exists
                sh 'rm -rf /home/jenkins/web'
                // Create the directory
                sh 'mkdir -p /home/jenkins/web'
            }
        }

        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'Dropping the container if exists...'
                sh 'docker rm -f apache1 || true'
            }
        }

        stage('Create the Apache HTTPD container') {
            steps {
               echo 'Creating the container with curl installed...'
        sh '''
        docker run -dit --name apache1 \
            -p 9000:80 \
            -v /home/jenkins/web:/usr/local/apache2/htdocs/ \
            httpd:latest bash -c "apt-get update && apt-get install -y curl && httpd-foreground"
        '''
    
            }
        }

        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'
                sh 'cp -r web/* /home/jenkins/web'
            }
        }

        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                // Using curl inside container to check Apache
                sh 'docker exec apache1 curl -I http://localhost || exit 1'
                echo 'App is reachable!'
            }
        }
    }
}
