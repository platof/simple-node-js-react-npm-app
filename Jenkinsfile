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
        stage('push image') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub_login', passwordVariable: 'docker_hub_loginPassword', usernameVariable: 'docker_hub_loginUser' )]) {
                    sh "docker login -u ${env.docker_hub_loginUser} -p ${env.docker_hub_loginPassword}"
                    sh 'docker push platof/my-react-app'
                }
            }
        }
            
        
    }
}
