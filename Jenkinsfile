pipeline {
    agent { label "dev-server"}
    
    stages {
        
        stage("code"){
            steps{
                git url: "https://github.com/bhavishaysikka/node-todo-cicd.git", branch: "master"
                echo 'Code has been Cloned'
            }
        }
        
        
        
        stage("Build and Test"){
            steps{
                sh "docker build -t node-app ."
                echo 'Code has been built'
            }
        }

        
        stage("Push to DockerHub"){
            steps{
                withCredentials([usernamePassword(credentialsId:"dockerHub",passwordVariable:"dockerHubPass", usernameVariable:"dockerHubUser")]){
                sh "docker login -u ${env.dockerHubUser} -p ${env.dockerHubPass}"
                sh "docker tag node-app:latest ${env.dockerHubUser}/node-app-test-new:latest"
                sh "docker push ${env.dockerHubUser}/node-app-test-new:latest"
                echo 'Code pushed'
                }
            }
        }
        
        stage("Deploy"){
            steps{
                sh ("docker-compose down && docker-compose up -d")
                echo 'Code deployed successfully'
            }
        }
    }
}
