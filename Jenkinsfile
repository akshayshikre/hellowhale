podTemplate(label: 'mypod', containers: [
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.10.10', command: 'cat', ttyEnabled: true)
    //containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]
  ) {
    node('mypod') {
        git url: 'https://github.com/akshayshikre/hellowhale.git', branch: 'master'
        stage('Check running containers') {
            container('docker') {
                // example to show you can run docker commands when you mount the socket
                sh 'echo master13.'
                sh 'hostname'
                //sh 'hostname -i'
                //sh 'docker ps'
                //sh 'IMAGE_NAME=akshayshikre/hellowhale:${BUILD_NUMBER}'
              //sh 'docker build -t akshayshikre/hellowhale:master_${BUILD_NUMBER} .'
                sh 'docker images'
              //sh 'docker login -u akshayshikre -p Sqr@12345'
              //sh 'docker push akshayshikre/hellowhale:master_${BUILD_NUMBER}'
            }
        }
        stage('Test') {
            container('kubectl') {
                // example to show you can run docker commands when you mount the socket
                sh 'hostname'
                //sh 'kubectl get pods'
            }
        }
        stage('List pods') {
              withKubeConfig([credentialsId: 'kubeconfdirect',
                    caCertificate: '<ca-certificate>',
                    serverUrl: '<api-server-address>',
                    contextName: '<context-name>',
                    clusterName: '<cluster-name>'
                    ]) {
                        sh 'kubectl get pods'
                       }
       }
        
    }
}
