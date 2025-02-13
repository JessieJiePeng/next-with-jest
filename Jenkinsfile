pipeline {
    // use agent any if you don't have multiple nodes installed
    agent {
        label 'built-in'
    }

    stages {
        stage('Git checkout') {
            steps{
                git branch:'master', url:'https://github.com/JessieJiePeng/next-with-jest.git'
            }
        }
        
        stage('npm install') {
            steps{
                    sh '''
                    ls
                    npm install'''
            }
        }
        
        stage('Tests') {
            steps{
                    sh 'npm test'
            }
        }
        
        stage('Build Image') {
            steps {
                sh 'sudo docker build -t jessiepeng66/sample-app:jks .'
            }
        }
        
        stage('Publish Image') {
            steps {
                withCredentials([usernamePassword(credentialsId: 'dockerhub_jessie', passwordVariable: 'PASSWORD', usernameVariable: 'USERNAME')]) {
                    sh '''
                        sudo docker login -u $USERNAME -p $PASSWORD
                        sudo docker image push jessiepeng66/sample-app:jks
                    '''
                }
            }
        } 
        
        stage('Deploy Image') {
            steps {
                    sh '''
                        sudo usermod -a -G docker jenkins
                        docker rm -f sample-app-container 2>/dev/null || true
                        docker run -d --name sample-app-container -p 3000:3000 jessiepeng66/sample-app:jks
                    '''
            }
        } 
        
    }
    
    post {
      always {
        cleanWs()
      }
    }
}
