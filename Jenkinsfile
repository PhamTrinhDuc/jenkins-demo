pipeline {
  agent any

  options {
      buildDiscarder(logRotator(numToKeepStr: '5', daysToKeepStr: '5'))
      timestamps()
  }

  environment {
      registry = 'duc8504/jenskins-demo'      // DockerHub repo
      registryCredential = 'dockerhub'        // ID trong Jenkins Credential
      imageTag = "${env.BUILD_NUMBER}"        // Tag theo build number
  }

  stages {
    stage('Clone Code') {
      steps {
          git url: 'https://github.com/PhamTrinhDuc/jenkins-demo.git', branch: 'main'
      }
    }

    stage('Install & Test') {
      agent {
          docker {
              image 'python:3.9'
          }
      }
      steps {
          sh '''
          pip install --upgrade pip
          pip install -r requirements.txt
          pytest tests/
          '''
      }
    }

    stage('Build Docker Image') {
        steps {
            script {
                echo 'Building Docker image...'
                dockerImage = docker.build("${registry}:${imageTag}")
            }
        }
    }

    stage('Push to DockerHub') {
      steps {
        script {
          echo 'Pushing Docker image...'
          docker.withRegistry('', registryCredential) {
            dockerImage.push()
            dockerImage.push('latest')
          }
        }
      }
    }
  }

  post {
    always {
        cleanWs()
    }
    failure {
        echo 'Build failed 😢'
    }
    success {
        echo 'Build & Push thành công 🚀'
    }
  }
}
