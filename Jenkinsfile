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
                     caCertificate: '-----BEGIN CERTIFICATE-----
MIICyDCCAbCgAwIBAgIBADANBgkqhkiG9w0BAQsFADAVMRMwEQYDVQQDEwprdWJl
cm5ldGVzMB4XDTE4MTIwNjExNDEwMloXDTI4MTIwMzExNDEwMlowFTETMBEGA1UE
AxMKa3ViZXJuZXRlczCCASIwDQYJKoZIhvcNAQEBBQADggEPADCCAQoCggEBAKC6
sB5UBuhGm/shG6G9hYnM9hB9DQIBguz49cSy+Au8vl5AmWqkNrRIogJwJCtnEwpX
gkJrc6CsIqQtm+Ea1gEL560vxm6SW5NlNu0qTQWQ4LykvpPBOXEGhR4AavQPMCvV
k8uHhoaMjc4jw12a0egh1Lhd7/nZLS0eG7xW+w8GS54S5wBTkT3GbZ4DioRMqpW4
TWK9BSATmUVt9cVTVr77xeAuKTF0Gahel/tvFxhkv6CDDUsqnRTwJl15QeWnCG8o
BDUqtmqUY47sa+DLtMw577R9QNMjPziiLxmHNmEDiusi47yHeUYW/9MJtznh1tDd
wM+M0bc/clhAfIMsoQ8CAwEAAaMjMCEwDgYDVR0PAQH/BAQDAgKkMA8GA1UdEwEB
/wQFMAMBAf8wDQYJKoZIhvcNAQELBQADggEBADMi/9HAXOm55PFTCfkxqNzbCQrq
WHghmGbwOjTXImTVp2hi2uqpzgy83fafWntq+xkFbPk5HhFRzXmcDgCq7FP4+PBS
QhG05vFuA+VqgYyR75zfDGUpQ7Y7tlkuBCZHwOB8hBFx5ukjAKNibg6cHOCJOzQI
Ftyv0pR3z9nyuU4I6BzuNvWoub1byDD4qm0TO3Pq4vbpcJSiRffsNkEzofr0kxT2
IoeCD5DgZb2dj9uPkQyh26OVyZq/PumkDEUdxZeHj7wIrQH6Katg0uwAyXUhgM/V
M79cwJKawxL1eregIPe5/Sp4asfdqxDqnbFxU9z4XqZmL9kbFGjsxJBidVA=
-----END CERTIFICATE-----',
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
