node('slave-jnlp') {
    stage('Clone') {
        echo "1.Clone Stage"
        git url: "https://github.com/wrre/test-jenkins.git"
        script {
            tag = sh(returnStdout: true, script: 'git rev-parse --short HEAD').trim()
            appName = "hello-world"
            registryHost = "172.20.10.7:5000/"
            imageName = "${registryHost}${appName}:${tag}"
        }
    }
    stage('Build') {
        echo "2.Build Stage"
        sh "docker build -t ${imageName} ."
    }
    stage('Push') {
        echo "3. Push Stage"
        sh "docker push ${imageName}"
    }
    stage('Deploy') {
        echo "4. Deploy Stage"
        sh "sed -i 's/<IMAGE_NAME>/${imageName}/g' deployment.yaml"
        sh "kubectl apply -f deployment.yaml"
    }
}
