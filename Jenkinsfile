properties([
    pipelineTriggers([
        pollSCM('H/2 * * * *')
    ])
])
podTemplate(
    label: 'sandi-metz-enforcer-pod', 
    containers: [
        containerTemplate(
            name: 'sandi-metz-enforcer-container', 
            image: 'registry2.swarm.devfactory.com/codenation/sandimetz-enforcer:v1.0.2', 
            ttyEnabled: true, 
          command: 'cat'
        )
]) {
        node('sandi-metz-enforcer-pod') {
            stage('Checks') {
                container('sandi-metz-enforcer-container') {
                    def repositoryUrl = scm.getUserRemoteConfigs()[0].getUrl()
                    echo "Validating rules on ${repositoryUrl}:${env.BRANCH_NAME}"
                    sh "REPO_URL=${repositoryUrl} BRANCH=${env.BRANCH_NAME} bash sandimetz.enforcer.sh"
                }
            }
        }
    }
