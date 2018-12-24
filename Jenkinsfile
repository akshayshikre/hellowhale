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
        //stage('Clone repository') {
            // container('git') {
            //     sh 'whoami'
            //     sh 'hostname -i'
            //     sh 'git clone -b master https://github.com/akshayshikre/hellowhale.git'
            // }
        //}
        stage('Check running containers') {
            container('docker') {
                // example to show you can run docker commands when you mount the socket
                sh 'hostname'
                sh 'hostname -i'
                sh 'docker ps'
                sh 'IMAGE_NAME=akshayshikre/hellowhale:${BUILD_NUMBER}'
                sh 'ls -a'
                sh 'pwd'
                // dir('hellowhale/') {
                        sh 'ls -a'
                        sh 'pwd'
                    //    sh 'docker build -t akshayshikre/hellowhale:${BUILD_NUMBER} .'
                        sh 'docker images'
                    //    sh 'docker login -u akshayshikre -p Sqr@12345'
                   //     sh 'docker push akshayshikre/hellowhale:${BUILD_NUMBER}'
                // }
            }
        }
        stage('Test') {
            container('kubectl') {
                // example to show you can run docker commands when you mount the socket
                sh 'hostname'
                sh 'hostname -i'
               // sh 'kubectl get po'
            }
        }
        
        
    }
}
