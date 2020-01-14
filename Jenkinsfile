  
pipeline {
  
  environment {
    CLUSTER = "standard-cluster-1"
    CLUSTER_ZONE = "us-central1-a"
    JENKINS_CRED = "rails-lab-264600"
  }

  agent {
    kubernetes {
      label 'pipeline'
      defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: cd-jenkins
  containers:
  - name: gcloud
    image: gcr.io/cloud-builders/gcloud
    command:
    - cat
    tty: true
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true
"""
}
  }

  stages {
    
    stage("Checkout code") {
      steps {
        checkout scm
      }
    }
    stage('Build and push image with Container Builder') {
      steps {
        container('gcloud') {
          sh "PYTHONUNBUFFERED=1 gcloud builds submit --config cloudbuild.yaml ."
        } 
      }
    }

    stage('Deploy with Container Builder') {
      steps{
        container('kubectl') {
          sh 'kubectl apply -f k8s/deploy/'
        }
      }
    }

  }

}
