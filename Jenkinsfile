node {
    checkout scm

    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-world"
    registryHost = "172.20.10.7:5000/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stage "Build"
        git url: 'https://github.com/wrre/test-jenkins.git'

        agent {
            docker { image 'node:7-alpine' }
        }
    
        steps {
            sh 'docker build -t ${imageName}'
        }
        /*container('docker') {
            stage('Build code') {
                sh "docker build -t ${imageName}"
            }
        }*/

    stage "Push"
        git url: 'https://github.com/wrre/test-jenkins.git'
        
        agent {
            docker { image 'node:7-alpine' }
        }

        sh 'docker build -t ${imageName}'


        /*container('docker') {
            stage('Push docker') {
                sh "docker push ${imageName}"
            }
        }*/

    stage "Deploy"

        sh "sed 's#__IMAGE__#'$BUILDIMG'#' deployment.yml | kubectl apply -f -"
}
