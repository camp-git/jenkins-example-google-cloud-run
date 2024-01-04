pipeline {
  agent any
  environment {
    CLOUDSDK_CORE_PROJECT='my-project-55158-testing'
    CLIENT_EMAIL='jenkins-gcloud@my-project-55158-testing.iam.gserviceaccount.com'
    GCLOUD_CREDS=credentials('gcloiud')
  }
  stages {
    stage('Verify version') {
      steps {
        sh '''
          bat gcloud version
        '''
      }
    }
    stage('Authenticate') {
      steps {
        sh '''
          bat gcloud auth activate-service-account --key-file="$GCLOUD_CREDS"
        '''
      }
    }
    stage('Install service') {
      steps {
        sh '''
         bat gcloud run services replace service.yaml --platform='managed' --region='us-central1'
        '''
      }
    }
    stage('Allow allUsers') {
      steps {
        sh '''
         bat gcloud run services add-iam-policy-binding hello --region='us-central1' --member='allUsers' --role='roles/run.invoker'
        '''
      }
    }
  }
  post {
    always {
      sh 'bat gcloud auth revoke $CLIENT_EMAIL'
    }
  }
}
