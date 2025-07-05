pipeline {
    agent any
   
    stages {
        stage('Create web directory')
        {
            input {
              message 'Enter the data'
              parameters {
                    string(name:'AUTHOR', defaultValue: 'Dobri', description: 'Author of the web application deployment ')
                    string(name:'ENVIRONMENT', defaultValue: 'Development',description: 'Environment to deploy')
                 }
            }
            steps {
                echo "The responsible of this project is ${AUTHOR} and and will be deployed in ${ENVIRONMENT}"
                //Fisrt, drop the directory if exists
                // sh 'rm -rf /home/jenkins/web'
                
                //Create the directory
                sh 'sudo mkdir /home/cloud_user/web'  
            }
        }
        /* stage('Drop the Apache HTTPD Docker container'){
            steps {
            echo 'droping the container...'
            sh 'docker rm -f apache1'
            }
        } */
        
        stage('Create the Apache httpd container') {
            steps {
            echo 'Creating the container...'
            sh 'docker run -dit --name apache1 -p 9000:80  -v /home/cloud_user/web:/usr/local/apache2/htdocs/ httpd'
            }
        }
        stage('Copy the web application to the container directory') {
            steps {
                echo 'Copying web application...'             
                sh 'cp -r web/* /home/cloud_user/web'
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
