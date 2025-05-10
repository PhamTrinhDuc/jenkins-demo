pipeline {
  agent any 

  stages {
    stage('Clone Code') {
      steps {
        git url: 'https://github.com/PhamTrinhDuc/jenkins-demo.git', branch: 'main'      
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'pip3 install -r app/requirements.txt'
      }
    }

    stage('Run Tests') {
      steps {
        sh 'pytest app/tests'
      }
    }

    stage('Build Docker Image') {
      steps {
        sh 'docker build -t jenkins-demo:latest .'
      }
    }

    stage('Run Container') {
      steps {
        sh 'docker run -d -p 5000:5000 --name jenkins-demo-container jenkins-demo:latest'
      }
    }
  }
}
