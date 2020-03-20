

/*
    Create the kubernetes namespace
 */
def createNamespace (namespace) {
    echo "Creating namespace ${namespace} if needed"

    sh "[ ! -z \"\$(kubectl get ns ${namespace} -o name 2>/dev/null)\" ] || kubectl create ns ${namespace}"
}

/*
    Helm install
 */
def helmInstall (namespace, release) {
    echo "Installing ${release} in ${namespace}"

    script {
        release = "${release}-${namespace}"
        sh "helm repo add helm ${HELM_REPO}; helm repo update"
        sh """
            helm upgrade --install --namespace ${namespace} ${release} \
                --set imagePullSecrets=${IMG_PULL_SECRET} \
                --set image.repository=${DOCKER_REG}/${IMAGE_NAME},image.tag=${DOCKER_TAG} helm/acme
        """
        sh "sleep 5"
    }
}

/*
    Helm delete (if exists)
 */
def helmDelete (namespace, release) {
    echo "Deleting ${release} in ${namespace} if deployed"

    script {
        release = "${release}-${namespace}"
        sh "[ -z \"\$(helm ls --short ${release} 2>/dev/null)\" ] || helm delete --purge ${release}"
    }
}



/*
    This is the main pipeline section with the stages of the CI/CD
 */
pipeline {



    // In this example, all is built and run from the master
    agent { node { label 'master' } }

    // Pipeline stages
    stages {

        ////////// Step 1 //////////
        stage('Git clone and setup') {
            steps {
                echo "Check out acme code"
                git branch: "master",
                        
                        url: 'https://github.com/itsrivastava/kafka-helm.git'

                // Validate kubectl
                sh "kubectl cluster-info"

                // Init helm client
                sh "helm init"

            

                // Load Docker registry and Helm repository configurations from file
                load "${JENKINS_HOME}/parameters.groovy"

                echo "DOCKER_REG is ${DOCKER_REG}"
                echo "HELM_REPO  is ${HELM_REPO}"

                // Define a unique name for the tests container and helm release
                script {
                    branch = GIT_BRANCH.replaceAll('/', '-').replaceAll('\\*', '-')
                    ID = "${IMAGE_NAME}-${DOCKER_TAG}-${branch}"

                    echo "Global ID set to ${ID}"
                }
            }
        }

 

        ////////// Step 3 //////////
        stage('Publish  Helm') {
            steps {
               
                echo "Packing helm chart"
                sh "${WORKSPACE}/build.sh --pack_helm --push_helm --helm_repo ${HELM_REPO} --helm_usr ${HELM_USR} --helm_psw ${HELM_PSW}"
            }
        }

        ////////// Step 4 //////////
        stage('Deploy to dev') {
            steps {
                script {
                    namespace = 'development'

                    echo "Deploying application ${ID} to ${namespace} namespace"
                    createNamespace (namespace)

                    // Remove release if exists
                    helmDelete (namespace, "${ID}")

                    // Deploy with helm
                    echo "Deploying"
                    helmInstall(namespace, "${ID}")
                }
            }
        }

    }
}