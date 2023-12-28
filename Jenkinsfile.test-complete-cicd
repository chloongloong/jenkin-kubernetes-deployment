podTemplate(yaml: '''
    apiVersion: v1
    kind: Pod
    spec:
      containers:
        securityContext:
          runAsUser: 1000
      - name: kubectl
        image: bitnami/kubectl:latest
        command:
        - cat
        tty: true
      - name: kaniko
        image: gcr.io/kaniko-project/executor:debug
        command:
        - sleep
        args:
        - 9999999
        volumeMounts:
        - name: kaniko-secret
          mountPath: /kaniko/.docker
      restartPolicy: Never
      volumes:
      - name: kaniko-secret
        secret:
            secretName: cred-mydh
            items:
            - key: .dockerconfigjson
              path: config.json
''') {
  node(POD_LABEL) {

    stage('Checkout Source') {
	script {
                    git branch: 'main',
                        credentialsId: 'github-credential',
        		url: 'https://github.com/chloongloong/jenkins-kubernetes-deployment.git'
                }
    }

    stage('Build React-App Image') {
      container('kaniko') {
        stage('Build a NodeJs project') {
          sh '''
            pwd
            cat $BUILD_NUMBER
          '''
        }
      }
    }

    stage('test kubectl installation') {
      container('kubectl') {
        sh 'ls -lrt'
      	sh 'kubectl get pods'
      }
    }

  }
}
