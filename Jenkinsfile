podTemplate(containers: [
    containerTemplate(
        name: 'docker', 
        image: 'docker',
	command: 'sleep',
	args: '30d'
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
