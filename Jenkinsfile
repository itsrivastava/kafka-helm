pipeline {
  agent {
    kubernetes {
      label 'helm-pod'
      containerTemplate {
        name 'helm'
        image 'wardviaene/helm-s3'
        ttyEnabled true
        command 'cat'
      }
    }
  }
  stages {
    stage('Run helm') {
      steps {
        container('helm') {
          git branch: "master",
                   credentialsId: 'github',
                   url: 'https://github.com/itsrivastava/kafka-helm.git'
          sh '''
          
          PACKAGE=demo-chart
          
         
          cp -r /home/helm/.helm ~
          helm repo add helm http://34.67.48.75:32445/artifactory/helm-remote --username admin --password Welcome@123
          cd helm/${PACKAGE}
          helm dependency update
          helm package .
          helm push --force ${PACKAGE}-*.tgz my-charts
          '''
        }
      }
    }
  }
}
