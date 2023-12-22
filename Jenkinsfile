pipeline {

podTemplate(containers: [
    containerTemplate(
        name: 'docker', 
        image: 'docker'
        )
  ]) {

    node(POD_LABEL) {
        stage('docker build stage') {
            container('docker') {
                stage('Shell Execution') {
                    sh '''
                    echo "Hello! I am executing shell"
		    docker version
                    '''
                }
            }
        }

    }
}
}
