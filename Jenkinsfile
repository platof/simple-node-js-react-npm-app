pipeline {
    agent any
    
    stages {
        stage('Inital Build ') {
            steps {
                // Install the dependencies
                sh 'npm install'

                //Build the project
                sh 'npm run build'
            }
        }
        
        stage('Build docker image') {
            steps {
                script {
                    app = docker.build("platof/my-react-app")
                    app.inside {
                        sh 'echo "image built"'
                        }
                    
                    }
                }
        }

        stage('Push Docker image'){
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'docker_hub_login') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }

            
        
    }
}
