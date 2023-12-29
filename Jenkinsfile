pipeline{
    agent{
        label "jenkins-agent"
    }
    parameters {
        string(defaultValue: "", description: 'Build Number', name: 'IMAGE_TAG')
    }
    environment{
        imageName = "tahtoh/devops"
    }
    stages {
        stage("CleanUp Workspace"){
            steps{
                cleanWs()
            }
        }
        stage("Checkout from SCM"){
            steps{
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/tahtoh/argocd-complete-e2e-pipeline.git'
            }
        }
        stage('Update deployment tag'){
            steps{
                sh """
                    cat deployment.yaml
                    's/${imageName}.*/${imageName}:${IMAGE_TAG}/g' deployment.yaml
                    cat deployment.yaml
                """
            }
        }
        stage('Push changes to git for deployment'){
            steps{
                sh """
                    git add deployment.yaml
                    git commit -m "update deployment.yaml"
                """
                withCredentials([gitUsernamePassword(credentialsId: 'github', gitToolName: 'Default')]) {
                    sh "git push https://github.com/tahtoh/argocd-complete-e2e-pipeline.git main"
                }
            }
        }
    }   
}
