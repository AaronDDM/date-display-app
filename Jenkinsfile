def label = "worker-${UUID.randomUUID().toString()}"

podTemplate(label: label, containers: [
  containerTemplate(name: 'node', image: 'node:carbon-jessie', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
  containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
],
volumes: [
  hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock')
]) {
  node(label) {
    echo "Your Pipeline works!"

       stage('Checkout'){
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
         withCredentials([[$class: 'UsernamePasswordMultiBinding',
            credentialsId: 'dockerhub',
            usernameVariable: 'aarondmconvergene',
            passwordVariable: 'test123123']]) {
            sh """
               docker login -u ${DOCKER_HUB_USER} -p ${DOCKER_HUB_PASSWORD}
               docker build -t aarondmconvergene/team3:${gitCommit} .
               docker push aarondmconvergene/team3:${gitCommit}
               """
         }
         }
      }
  }
}
