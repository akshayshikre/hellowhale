podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.10.10', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]
  ) {
    node('mypod') {
        git url: 'https://github.com/akshayshikre/hellowhale.git', branch: 'dev'
        stage('Run docker') {
            container('docker') {
                // example to show you can run docker commands when you mount the socket
                sh 'echo dev13.'
                sh 'hostname'
                sh 'hostname -i'
                sh 'docker ps'
                sh 'IMAGE_NAME=akshayshikre/hellowhale:${BUILD_NUMBER}'
                sh 'docker build --no-cache -t akshayshikre/hellowhale:master_${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker login -u akshayshikre -p Sqr@12345'
                sh 'docker push akshayshikre/hellowhale:dev_${BUILD_NUMBER}'
            }
        }
        stage('Run kubectl') {
            container('kubectl') {
                
           //    withKubeConfig([credentialsId: 'kubeconfdirect',
           //          caCertificate: '<ca-certificate>',
           //          serverUrl: 'https://192.168.99.100:8443',
           //          contextName: 'minikube',
           //          clusterName: 'minikube'
           //          ]) {
                        sh 'hostname'
                        sh 'kubectl config view'
                        sh 'kubectl version'
                        sh 'kubectl get pods -n default'
                        sh 'kubectl set image deployment/hellowhale -n default hellowhale=akshayshikre/hellowhale:dev_${BUILD_NUMBER}'
                        sh 'sleep 30'
                        sh 'kubectl rollout history -n default deployment/hellowhale'
           //            }
            }
       }
        
      stage('Run helm') {
         container('helm') {
                  //sh "helm list"
                  }
        }
        
    }
}
