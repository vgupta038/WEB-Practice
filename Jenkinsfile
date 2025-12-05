pipeline {
    agent any
   
    stages {
        stage('Create web directory')
        {
            input {
              message 'Enter the data'
              parameters {
                    string(name:'AUTHOR', defaultValue: 'Sergio', description: 'Author of the web application deployment ')
                    string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
                 }
            }
            steps{
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
                //Fisrt, drop the directory if exists
                //sh 'rm -rf /home/jenkins/web'
                //Create the directory
                //sh 'mkdir /home/jenkins/web'
               // sh 'powershell.exe mkdir C:\\Users\\vgupt\\web'                
            }
        }
        stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        }
        stage('Create the Apache httpd container') {
            steps {
            echo 'Creating the container...'
            //sh 'docker run -dit --name apache1 -p 9000:80  -v /home/jenkins/web:/usr/local/apache2/htdocs/ httpd'
             sh 'docker run -dit --name apache1 -p 9000:80 -v /mnt/c/Users/vgupt/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
               // sh 'cp -a web/. /home/jenkins/web/'
                //echo "After copy:"
                //sh 'ls -R /home/jenkins/web'
               // sh 'cp -a web/. /c/Users/vgupt/web/'
                 sh 'cp -a web/. /mnt/c/Users/vgupt/web/'
                 sh 'ls -R /mnt/c/Users/vgupt/web'
            }
        }
        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'wget http://localhost:9000'
            }
        }       
    }
}
