pipeline {

  environment {
    PROJECT = "demo2-248908"
    APP_NAME = "hello-app"
    FE_SVC_NAME = "${APP_NAME}-frontend"
    CLUSTER = "demo2-gke-cluster"
    CLUSTER_ZONE = "europe-west3-a"
    IMAGE_TAG = "eu.gcr.io/${PROJECT}/${APP_NAME}:${env.BUILD_NUMBER}"
    JENKINS_CRED = "${PROJECT}"
    BUILD_HOME='/var/lib/jenkins/workspace'
    APP_REPO="https://github.com/c4n2012/hello-app.git"
  }

 agent {
    kubernetes {
    //   label 'hello-app'
    //   defaultContainer 'jnlp'
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  serviceAccountName: jenkins-sa
  volumes:
  - name: dockersock
    hostPath:
      path: /var/run/docker.sock
  containers:
  - name: git
    image: gcr.io/cloud-builders/git
    command:
    - cat
    tty: true
  - name: golang
    image: golang:1.10
    command:
    - cat
    tty: true
  - name: docker
    image: gcr.io/cloud-builders/docker
    command:
    - cat
    tty: true
    volumeMounts:
    - name: dockersock
      mountPath: /var/run/docker.sock
  - name: kubectl
    image: gcr.io/cloud-builders/kubectl
    command:
    - cat
    tty: true

"""
}
  }
    stage('Checkout') {
        steps {
            git branch: 'master', url: "${APP_REPO}"
        }
    }
//   stages {
//     stage('Clone project from git repo') {
//       steps {
//         container('git') {
//           checkout scm
//         }
//       }
//     }
    stage('Build and push container') {
      steps {
        container('docker') {
        //  sh "cd $WORKSPACE/repo/${APP_NAME}";
         sh "docker build -t ${IMAGE_TAG} .";
         sh "docker images";
         sh "docker push ${IMAGE_TAG}";
        }
    } 
}

}
}