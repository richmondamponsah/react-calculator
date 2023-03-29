pipeline {
  agent any
  
  environment {
    SSH_KEY = credentials('ubuntu')
    SERVER = '127.0.0.1:8082'
    USERNAME = 'parallels@linsvr'
    PROJECT_NAME = 'calculator'
  }
  
  stages {
    stage('Checkout') {
      steps {
        git url: 'https://github.com/richmondamponsah/react-calculator.git', branch: 'main'
      }
    }
    
    stage('Install Dependencies') {
      steps {
        sh 'npm install'
      }
    }
    
    stage('Build') {
      steps {
        sh 'npm run build'
      }
    }
    
    stage('Deploy') {
      steps {
        withCredentials([sshUserPrivateKey(credentialsId: 'ssh-key', keyFileVariable: 'SSH_KEY')]) {
          sh """
            ssh -o StrictHostKeyChecking=no -i ${SSH_KEY} ${USERNAME}@${SERVER} 'mkdir -p /var/www/${PROJECT_NAME}'
            scp -o StrictHostKeyChecking=no -i ${SSH_KEY} -r build/* ${USERNAME}@${SERVER}:/var/www/${PROJECT_NAME}
          """
        }
      }
    }
  }
}
