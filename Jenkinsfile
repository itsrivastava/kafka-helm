podTemplate(
    label: 'mypod', 
    inheritFrom: 'default',
    containers: [
        containerTemplate(
            name: 'docker', 
            image: 'docker:18.02',
            ttyEnabled: true,
            command: 'cat'
        ),
        containerTemplate(
            name: 'helm', 
            image: 'ibmcom/k8s-helm:v2.6.0',
            ttyEnabled: true,
            command: 'cat'
        )
    ],
    volumes: [
        hostPathVolume(
            hostPath: '/var/run/docker.sock',
            mountPath: '/var/run/docker.sock'
        )
    ]
) {
    node('mypod') {
        def commitId
        stage ('checkout code') {
            git branch: "master",
                  credentialsId: 'eldada-bb',
                        url: 'https://github.com/eldada/jenkins-pipeline-kubernetes.git'
        }

    }
}
