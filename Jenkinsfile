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
        stage('Deploy to Production') {
            agent any
            steps {
                withCredentials([usernamePassword(credentialsId: 'docker_hub_login', passwordVariable: 'docker_hub_loginPassword', usernameVariable: 'docker_hub_loginUser' )]) {
                    script {
                        sh "sshpass -p '$docker_hub_loginPassword' -v ssh -o StrictHostKeyChecking=no vagrant@192.168.56.60 \"docker pull platof/my-react-app:${env.BUILD_NUMBER}\""
                        try {
                            sh "sshpass -p '$docker_hub_loginPassword' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker stop my-react-app\""
                            sh "sshpass -p '$docker_hub_loginPassword' -v ssh -o StrictHostKeyChecking=no $USERNAME@$prod_ip \"docker rm my-react-app\""
                        } catch (err) {
                            echo: 'caught error: $err'
                        }
                        sh "sshpass -p '$docker_hub_loginPassword' -v ssh -o StrictHostKeyChecking=no vagrant@192.168.56.60 \"docker run --restart always --name my-react-app -p 3000:3000 -d platof/my-react-app:${env.BUILD_NUMBER}\""
                    }
                }
            }
        }
            
        
    }
}
