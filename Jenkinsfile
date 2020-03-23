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
          stages{
            stage('run helm for kafka'){
              steps {
               container('kafka') {
                    
               
                    echo "Check out kafka code"
                    git url: 'https://github.com/itsrivastava/kafka-helm.git', branch: 'master', credentialsId: 'github'
                    sh '''
                    PACKAGE=kafka-chart
                    helm repo add helm http://34.67.48.75:32445/artifactory/helm-remote --username admin --password Welcome@123
                    cd helm/${PACKAGE}
                    helm dependency update
                    helm package .
                    
                    helm push --force ${PACKAGE}-*.tgz helm
                    DEPLOYED=$(helm list |grep -E "^${PACKAGE}" |grep DEPLOYED |wc -l)
                    if [ $DEPLOYED == 0 ] ; then
                      helm install --name ${PACKAGE} helm/${PACKAGE}
                    else
                      helm upgrade ${PACKAGE} helm/${PACKAGE}
                    fi
                    echo "deployed!"
                    '''
            }
            }}}
        }
    }
