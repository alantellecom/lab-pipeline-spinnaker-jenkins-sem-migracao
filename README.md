- Jenkins pipeline

Modificar campos Jenkinsfile:

  environment {
    CLUSTER = "standard-cluster-1"
    CLUSTER_ZONE = "us-central1-a"
    JENKINS_CRED = "rails-lab-264600"
  }

  Eliminar linha de copia do arquivo cludbuild:

  "- name: 'gcr.io/cloud-builders/gsutil'
  args: ['cp', '-r', 'k8s/*', 'gs://$PROJECT_ID-kubernetes-manifests']"

- Spinnaker pipeline

  "name": "Deploy",
  "application": "myapp",

  Modificar caminho do bucket onde manifest foram copiados via cloudbuild. A convenção é trocar o nome do projeto via replace do vs-code e nome dos manifests.

  "name": "gs://rails-lab-264600-kubernetes-manifests/deploy/deploy-prod.yml",
  "reference": "gs://rails-lab-264600-kubernetes-manifests/deploy/deploy-prod.yml",

  Modificar nome do projeto e nome da imagem que recebe nome da aplicação via cloudbuild. Trocar nome da versão default

  "name": "gcr.io/rails-lab-264600/sample-app",
  "reference": "gcr.io/rails-lab-264600/sample-app:v1",
  "version": "v1"



 
