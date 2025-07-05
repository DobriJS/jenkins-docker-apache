pipeline {
    agent any

    stages {
        stage('Create web directory') {
            steps {
                script {
                    def userInput = input(
                        message: 'Enter the data',
                        parameters: [
                            string(name: 'AUTHOR', defaultValue: 'Dobri', description: 'Author'),
                            string(name: 'ENVIRONMENT', defaultValue: 'Development', description: 'Environment')
                        ]
                    )
                    env.AUTHOR = userInput['AUTHOR']
                    env.ENVIRONMENT = userInput['ENVIRONMENT']
                }

                echo "The responsible of this project is ${env.AUTHOR} and will be deployed in ${env.ENVIRONMENT}"

                sh 'rm -rf web'
                sh 'mkdir web'
            }
        }

        stage('Drop the Apache HTTPD Docker container') {
            steps {
                echo 'Dropping the container...'
                sh 'docker rm -f apache1 || true' // prevent failure if it doesn't exist
            }
        }

        stage('Create the Apache httpd container') {
            steps {
                echo 'Creating the container...'
                sh 'docker run -dit --name apache1 -p 9000:80 -v $(pwd)/web:/usr/local/apache2/htdocs/ httpd'
            }
        }

        stage('Checking the app') {
            steps {
                echo 'Testing the web app'
                sh 'sleep 5' // wait a bit for the container to start
                sh 'curl --fail --silent http://localhost:9000 || echo "App not responding"'
            }
        }
    }
}

