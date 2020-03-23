pipeline {
  agent {
    kubernetes {
      label 'kafka-pod'
      containerTemplate {
        name 'kafka-pod'
        image 'dtzar/helm-kubectl'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
    stage('Run helm') {
      steps {
        container('kafka-pod') {
                    echo "Check out kafka code"
                    git url: 'https://github.com/itsrivastava/kafka-helm.git', branch: 'master', credentialsId: 'github'
                    sh '''
                    PACKAGE=kafka-chart
                    helm repo add helm http://34.67.48.75:32445/artifactory/helm-remote --username admin --password Welcome@123
                    
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
      }
    }
  }
}
