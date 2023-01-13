def IMG = "192.168.100.12/commerce-yr/commerce-yr-person-img"
def KC = "/usr/local/bin/kubectl --kubeconfig=/home/jenkins/acloud-client.conf"

pipeline {

  environment {
    registry = "lee-code712/Commerce-Person"
    dockerImage = ""
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        echo "Checkout Source START"
        git 'https://github.com/lee-code712/Commerce-Person.git'
        echo "Checkout Source END"
      }
    }

    stage('Build image') {
      steps{
        script {
          echo "Build image START $BUILD_NUMBER"
          sh "docker build --no-cache -t ${IMG}:person-${BUILD_NUMBER} ."
          echo "Build image END"
        }
      }
    }

    stage('Push Image') {
      environment {
               registryCredential = 'harbor'
           }
      steps{
        script {
          echo "Push Image START"
          sh "docker login 192.168.100.12 -u admin -p Unipoint11"
          sh "docker push ${IMG}:person-${BUILD_NUMBER}"
          }
        echo "Push Image END"
      }
    }
    

    stage('Deploy App') {
      steps {
        script {
          echo "Deploy App START"
          sh "${KC} apply -f person_deployment_v2.yaml"
          sh "${KC} set image deployment/commerce-yr-person-v2 commerce-yr-person=${IMG}:person-${BUILD_NUMBER} -n commerce-yr"
          echo "Deploy App END"
        }
      }
    }

  }

}
