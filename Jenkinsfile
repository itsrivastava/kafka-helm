podTemplate(
    label: 'kafka', 
    inheritFrom: 'default',
    containers: [
        containerTemplate(
            name: 'kafka', 
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
    node('kafka') {
        def commitId
        stage ('checkout code') {
            git branch: "master",
                  credentialsId: 'github',
                  url: 'https://github.com/itsrivastava/kafka-helm.git'
        }

    }
}
