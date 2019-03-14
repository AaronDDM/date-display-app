def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'node', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.8.8', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    echo "Your Pipeline works!"

       /*stage('Checkout'){
          checkout scm
       }
       
       stage('Test') {
         try {
            container('node') {
               env.NODE_ENV = "test"

               print "Environment will be : ${env.NODE_ENV}"

               sh 'node -v'
               sh 'npm prune'
               sh 'npm install'
               sh 'npm test'
            }
         } catch (exc) {
            println "Failed to test - ${currentBuild.fullDisplayName}"
            throw(exc)
         }
       }


      stage('Create Docker images') {
         container('docker') {
            sh """
               docker login -u aarondmconvergence -p test123123
               docker build -t aarondmconvergence/team3:latest .
               docker push aarondmconvergence/team3:latest
               """
         }
      }*/

      stage('Run kubectl') {
         container('kubectl') {
            sh """
               kubectl get namespaces
               kubectl cluster-info
               """
         }
      }

      stage('Deploy') {
         container('kubectl') {
            sh "kubectl get serviceaccount"
            //sh "kubectl run --namespace=jenkins-team3 --image=aarondmconvergence/team3:latest team3"
         }
      }

      stage('Run kubectl x2') {
         container('kubectl') {
            sh "kubectl get pods"
         }
      }
  }
}
