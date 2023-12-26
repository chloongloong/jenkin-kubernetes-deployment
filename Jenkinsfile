pipeline {
  agent {
    kubernetes {
      yaml '''
        apiVersion: v1
        kind: Pod
        metadata:
          namespace: jenkins
          labels:
            app: jenkins-ci
        spec:
          securityContext:
            runAsUser: 1000
          containers:
          - name: kubectl
            image: bitnami/kubectl:latest
            command:
            - cat
            tty: true
        '''
    }
  }

stages{
        stage('test kubectl installation') {
      	 steps {
           container('kubectl') {
        	sh 'kubectl get pods' 
       	   } 	
         }
  	}
}
}
