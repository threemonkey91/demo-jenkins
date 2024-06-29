pipeline {
   agent any
   environment {
      PROJECT = 'WELCOME TO K8S B28 BATCH - Jenkins & AWS EKS Class'
      CLUSTER_NAME = 'k8sb28-cluster-01'
      DESTROY = 'FALSE'
   }
   stages {
      stage('Check The Kubernetes Access') {
         steps {
            sh "aws eks --region us-east-2 update-kubeconfig --name ${CLUSTER_NAME}"
            sh 'kubectl get pods -A'
            sh 'kubectl get ns'
         }
      }
      stage('Deploy Voting App') {
         steps {
            sh 'ls -al'
            sh 'kubectl apply -f voting-with-ingress.yml'
            sh 'kubectl apply -f ingress.yml'
            sh 'kubectl apply -f ingress-nk.yml'
            sh 'kubectl get pods,svc,ingress'
         }
      }
      stage('Deploy Ingress for Vote & Result') {
         steps {
            sh 'kubectl apply -f ingress.yml'
            sh 'kubectl get ingress'
         }
      }
      stage('Validating Deployment') {
         steps {
            sh 'kubectl get pods,deployment,svc,ing'
         }
      }
      stage('Destroy') {
         when {
            expression {
               "${env.DESTROY}" == 'TRUE'
            }
         }
         steps {
            sh 'ls -al'
            sh 'kubectl delete -f voting-with-ingress.yml'
            sh 'kubectl delete -f ingress.yml'
            sh 'kubectl delete -f ingress-nk.yml'
            sh 'kubectl get pods,svc,ingress'
         }
      }
   }
}
