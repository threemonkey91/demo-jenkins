pipeline {
   agent any
   environment {
      PROJECT = 'WELCOME TO K8S B27 BATCH - Jenkins & AWS EKS Class'
      CLUSTER_NAME = 'k8sb27-cluster-01'
      DESTROY = 'TRUE'
   }
   stages {
      stage('Check The Kubernetes Access') {
         steps {
            sh "aws eks --region us-east-2 update-kubeconfig --name ${CLUSTER_NAME}"
            sh 'kubectl get pods -A'
            sh 'kubectl get deploy'
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
         when {
            expression {
               "${env.DESTROY}" == 'FALSE'
            }
         }
         steps {
            sh 'kubectl apply -f ingress.yml'
            sh 'kubectl get ingress'
         }
      }
      stage('Validating Deployment') {
         when {
            expression {
               "${env.DESTROY}" == 'FALSE'
            }
         }
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
