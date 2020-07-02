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
            agent any
            steps {
                sh 'docker build -t platof/my-react-app .'
                }
        }
            
        
    }
}
