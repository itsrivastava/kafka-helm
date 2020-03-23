podTemplate(
    name: 'kafka-pod',
    label: 'kafka-pod',
    containers: [
        containerTemplate(name: 'kafka', image: 'dtzar/helm-kubectl'),
        containerTemplate(name: 'docker', image:'trion/jenkins-docker-client'),
    ])
    {
        //node = the pod label
        node('kafka-pod'){
            //container = the container label
            stage('checkout scm'){
                steps {
                    echo "Check out kafka code"
                    git branch: "master",
                        credentialsId: 'github',
                        url: 'https://github.com/itsrivastava/kafka-helm.git'
            }
            }
        }
    }
