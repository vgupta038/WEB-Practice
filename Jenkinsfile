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
                sh 'rm -rf /home/jenkins/web'
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
                echo 'Creating the Apache HTTPD container...'
                // Start the container normally without installing anything
                sh '''
                docker run -dit --name apache1 \
                    -p 9000:80 \
                    -v /home/jenkins/web:/usr/local/apache2/htdocs/ \
                    httpd:latest
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
                echo 'Testing the web app from Jenkins host...'
                // Check Apache from Jenkins container/host using wget
                sh '''
                for i in {1..10}; do
                    if wget --spider -q http://localhost:9000; then
                        echo "App is reachable!"
                        exit 0
                    else
                        echo "Waiting for Apache to start..."
                        sleep 3
                    fi
                done
                echo "App is NOT reachable!"
                exit 1
                '''
            }
        }
    }
}
