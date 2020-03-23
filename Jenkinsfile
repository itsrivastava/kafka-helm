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
                    helm plugin install https://github.com/belitre/helm-push-artifactory-plugin --version v1.0.0
                    helm repo add helm http://34.67.48.75:32445/artifactory/helm-local --username admin --password Welcome@123
                    
                    helm dependency update
                    helm package .
                    helm list
                    helm push-artifactory kafka-7.3.1.tgz helm
                    echo "pushed in atrifactory"
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
