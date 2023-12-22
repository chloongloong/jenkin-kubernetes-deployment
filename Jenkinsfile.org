pipeline {

  environment {
    dockerimagename = "chloong/react-app"
    dockerImage = ""
  }

 agent {
    kubernetes {
      yaml """
apiVersion: v1
kind: Pod
metadata:
labels:
  component: ci
spec:
  # Use service account that can deploy to all namespaces
  # serviceAccountName: jenkins
  securityContext:
    fsGroup: 1000
    runAsUser: 1000
  containers:
  - name: docker
    image: docker:latest
    command:
    - cat
    tty: true
    volumeMounts:
    - mountPath: /var/run/docker.sock
      name: docker-sock
  volumes:
    - name: docker-sock
      hostPath:
        path: /var/run/docker.sock
"""
}
   }

  stages {

    stage('Checkout Source') {
      steps {
	script {
                    git branch: 'main',
                        credentialsId: 'github-credential',
        		url: 'https://github.com/chloongloong/jenkins-kubernetes-deployment.git'
                }
      }
    }

    stage('Build image') {
      steps{
	container('docker') {
          sh """
             docker build -t $dockerimagename:$BUILD_NUMBER .
          """
        }
      }
    }

    stage('Push image') {
      steps{
	container('docker') {
          sh """
             docker image tag $dockerimagename:$BUILD_NUMBER $dockerimagename:latest
	     docker image push $dockerimagename:latest
          """
        }
      }
    }

    stage('Deploying React.js container to Kubernetes') {
      steps {
        script {
          kubernetesDeploy(configs: "deployment.yaml", "service-lb.yaml")
        }
      }
    }

  }

}
