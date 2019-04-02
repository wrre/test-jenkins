node('slave-jnlp') {
    stage('Clone') {
        script {
            imageName = "172.20.10.7:5000/hello-world:cccde00"
        }
    }
    stage('Deploy') {
        echo "4. Deploy Stage"
        sh 'sed -i "s/<IMAGE_NAME>/${imageName}/g\" deployment.yml'
        sh "kubectl apply -f deployment.yml"
    }
}
