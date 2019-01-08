def label = "worker-${UUID.randomUUID().toString()}"
podTemplate(label: label, containers: [
    containerTemplate(name: 'docker', image: 'docker', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'kubectl', image: 'lachlanevenson/k8s-kubectl:v1.10.10', command: 'cat', ttyEnabled: true),
    containerTemplate(name: 'helm', image: 'lachlanevenson/k8s-helm:latest', command: 'cat', ttyEnabled: true)
  ],
  volumes: [
    hostPathVolume(mountPath: '/var/run/docker.sock', hostPath: '/var/run/docker.sock'),
  ]
  ) {
    node(label) {
        git url: 'https://github.com/akshayshikre/hellowhale.git', branch: 'dev'
        //def myRepo = checkout scm
        //def gitCommit = myRepo.GIT_COMMIT
        //def gitBranch = myRepo.GIT_BRANCH
        //def shortGitCommit = "${gitCommit[0..10]}"
        //def previousGitCommit = sh(script: "git rev-parse ${gitCommit}~", returnStdout: true)

        stage('Run docker') {
            container('docker') {
                // example to show you can run docker commands when you mount the socket
                sh 'echo dev13.'
                sh 'hostname'
                sh 'hostname -i'
                sh 'docker ps'
                sh 'IMAGE_NAME=akshayshikre/hellowhale:dev_${BUILD_NUMBER}'
                sh 'docker build -t akshayshikre/hellowhale:dev_${BUILD_NUMBER} .'
                sh 'docker images'
                sh 'docker login -u ${DOCKER_USR} -p ${DOCKER_PWD}'
                sh 'docker push akshayshikre/hellowhale:dev_${BUILD_NUMBER}'
            }
        }
        stage('Run kubectl') {
            container('kubectl') {
                
               withKubeConfig([credentialsId: 'kubedefaultjenkins',
                     caCertificate: 'LS0tLS1CRUdJTiBDRVJUSUZJQ0FURS0tLS0tCk1JSUM4akNDQWRxZ0F3SUJBZ0lJVlF3OHlEdUljdWd3RFFZSktvWklodmNOQVFFTEJRQXdGVEVUTUJFR0ExVUUKQXhNS2EzVmlaWEp1WlhSbGN6QWVGdzB4T0RFeU1EWXhNVFF4TURKYUZ3MHhPVEV5TURZeE1UUXhNREphTURReApGekFWQmdOVkJBb1REbk41YzNSbGJUcHRZWE4wWlhKek1Sa3dGd1lEVlFRREV4QnJkV0psY201bGRHVnpMV0ZrCmJXbHVNSUlCSWpBTkJna3Foa2lHOXcwQkFRRUZBQU9DQVE4QU1JSUJDZ0tDQVFFQXlmbXlmbHlyK0hxZnNMU1UKZW4wRmhwNzkvejBSWUxFVFlVb2RKMFFrNWd3ek9jeDdldmZIbWlxTHpZNDhZSmZzOG5BWGF2UmVUeDJWQUlVTQp5bE5hT1YvN1pUY2RuLzFJUi9KVHJHT1U5bldSWTBNM1NoZGdHNDB1amx4VWY5VFYrVEpMN0Z3dE9lYWhtOFh4CkJOS2x0UWdnL2ZEUWFQSW8rdldPS0ZENGdMMC92WGF3N2xhYWR0VUtZSkEvNzlOUGV2Tndxa2Fac1AzTnJ3bTMKcXd5aHNwN0JId1QvUWhQdzNTUitHV3NhdWhFOGhCaklGOWxYa0c5dTJ3UTdFd01GdHZJR2FnOFJxVVNzK1BTZgpJK0labTlpQzVHb0hxK2toWlFhazFrWlJ6V2I2SU5obGY0dzcydXZTVER0a0VEc28rZFZRaDJ2U2JEVHpKYS9aCmhFT092d0lEQVFBQm95Y3dKVEFPQmdOVkhROEJBZjhFQkFNQ0JhQXdFd1lEVlIwbEJBd3dDZ1lJS3dZQkJRVUgKQXdJd0RRWUpLb1pJaHZjTkFRRUxCUUFEZ2dFQkFJenFpbjB3RzBRenhOTUJTOW85blZ3cFp5d3RwOXlmWjB6eApHUG8xNStORjJEMHIrV052VzdKQ0I5OXJzUmk5bXAxQ1JXY2dwKytmdFVaMWN3QXFYVHI5NG56M1VEVDBaaXRVCmU2eVRqdGVCWDVUajhQTWloaUVmT3dwQW9LeG40ZjRTa3pvdzhXQjl0bGdWcm0zdnV4VFBmc0xCR3dKdWlZTmcKMHJOMFFsQm1HUVZuQnVwS3Yva3pLUldJTWxpbjlybE9FSGxvSGZpUVByLzlSc0xLcVFORWRhVU9TblJzNGpUMwpQOVZIZGcvODQxQUVxU0hBMDhaSUdIWWFuVHR0NE9YRW5vdmg5cTZZYTZSVmw3L0ZjY3JqNkpoRmFJTEovQWplCnN1RFNCQ3A4M3ZNSmN1aStqcWsydTBNcnJOWi9qejNBdEhqT0g5bkhOOEZ5b1VvaUxqdz0KLS0tLS1FTkQgQ0VSVElGSUNBVEUtLS0tLQo=',
                     serverUrl: 'https://157.230.9.247:6443',
                     contextName: 'kubernetes-admin@kubernetes',
                     clusterName: 'kubernetes'
                     ]) {
                        sh 'hostname'
                        sh 'kubectl config view'
                        sh 'kubectl version'
                        sh 'kubectl get pods -n default'
                        sh 'kubectl set image deployment/hellowhale -n default hellowhale=akshayshikre/hellowhale:dev_${BUILD_NUMBER}'
                        sh 'sleep 30'
                        sh 'kubectl rollout history -n default deployment/hellowhale'
                       }
            }
       }
        
      stage('Run helm') {
         container('helm') {
                  //sh "helm list"
                    sh 'hostname'
                  }
        }
        
    }
}
