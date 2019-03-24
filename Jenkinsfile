podTemplate(label: 'jenkins-slave-pod', ,cloud: 'kubernetes', containers: [
        containerTemplate(name: 'docker', image: 'registry.cn-hangzhou.aliyuncs.com/spacexnice/jenkins-slave:latest', command: '', ttyEnabled: false)
    ]
    ,volumes: [
        hostPathVolume(hostPath: '/var/run/docker.sock', mountPath: '/var/run/docker.sock')
    ]) {
        node {
            checkout scm
            
            env.DOCKER_API_VERSION="1.23"

            sh "git rev-parse --short HEAD > commit-id"

            tag = readFile('commit-id').replace("\n", "").replace("\r", "")
            appName = "hello-world"
            registryHost = "172.20.10.7:5000/"
            imageName = "${registryHost}${appName}:${tag}"
            env.BUILDIMG=imageName
                
            sh 'echo ===1'

            stage "Build"
                sh 'echo ===2'
                git url: 'https://github.com/wrre/test-jenkins.git'
                
                sh 'echo ===3'

                container('docker') {
                    stage('Build code') {
                        sh 'echo ===4'
                        sh "docker build -t ${imageName}"
                        sh 'echo ===5'
                    }
                }

            stage "Push"
                git url: 'https://github.com/wrre/test-jenkins.git'

                container('docker') {
                    stage('Push docker') {
                        sh "docker push ${imageName}"
                    }
                }

            stage "Deploy"

                sh "sed 's#__IMAGE__#'$BUILDIMG'#' deployment.yml | kubectl apply -f -"
        }
}
