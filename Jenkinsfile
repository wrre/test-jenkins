pipeline {
    /*checkout scm*/

    sh "git rev-parse --short HEAD > commit-id"

    tag = readFile('commit-id').replace("\n", "").replace("\r", "")
    appName = "hello-world"
    registryHost = "172.20.10.7:5000/"
    imageName = "${registryHost}${appName}:${tag}"
    env.BUILDIMG=imageName

    stages {
        stage ("Build") {
            /*git url: 'https://github.com/wrre/test-jenkins.git'*/

            agent {
                docker { image 'docker' }
            }
        
            steps {
                sh 'echo ===1'
                sh 'docker build -t ${imageName}'
                sh 'echo ===2'
            }
            /*container('docker') {
                stage('Build code') {
                    sh "docker build -t ${imageName}"
                }
            }*/
        }


        stage ("Push") {
            /*git url: 'https://github.com/wrre/test-jenkins.git'*/
            
            agent {
                docker { image 'docker' }
            }

            steps {
                sh 'echo ===3'
                sh 'docker build -t ${imageName}'
                sh 'echo ===4'
            }

            /*container('docker') {
                stage('Push docker') {
                    sh "docker push ${imageName}"
                }
            }*/
        }
            

        stage ("Deploy") {
            steps {
                sh "sed 's#__IMAGE__#'$BUILDIMG'#' deployment.yml | kubectl apply -f -"
            } 
        }
    }
}
