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
          containers:
          - name: kubectl
            image: bitnami/kubectl:latest
            securityContext:
              runAsUser: 1000
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
                sh 'pwd'
        	sh 'kubectl get pods' 
       	   } 	
         }
  	}
}
}
