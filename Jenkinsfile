pipeline {
  agent {
    docker {
      image 'python:3.9' // Image có Python 3.9 và pip
      args '-v /var/run/docker.sock:/var/run/docker.sock' // Cho phép dùng Docker
    }
  }
  
  options{
        // Max number of build logs to keep and days to keep
        buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
        // Enable timestamp at each job in the pipeline
        timestamps()
    }

    // environment{
    //     registry = 'quandvrobusto/house-price-prediction-api'
    //     registryCredential = 'dockerhub'      
    // }

  stages {
    stage('Clone Code') {
      steps {
        git url: 'https://github.com/PhamTrinhDuc/jenkins-demo.git', branch: 'main'      
      }
    }

    stage('Install Dependencies') {
      steps {
        sh 'pip install -r app/requirements.txt'
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
