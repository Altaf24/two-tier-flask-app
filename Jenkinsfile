pipeline {
    agent {label "dev"};

    stages {

        stage("Code") {
            steps {
                git url: "https://github.com/Altaf24/two-tier-flask-app.git", branch: "master"
            }
        }

        stage("Build") {
            steps {
                sh "docker build -t two-tier-flask-app ."
            }
        }

        stage("Test") {
            steps {
                echo "Developer / Tester tests likh dega"
            }
        }

        stage("Push To Docker Hub") {
            steps {
                withCredentials([usernamePassword(
                    credentialsId: 'dockerHubCreds',
                    usernameVariable: 'dockerHubUser',
                    passwordVariable: 'dockerHubPass'
                )]) {

                    sh "echo ${env.dockerHubPass} | docker login -u ${env.dockerHubUser} --password-stdin"
                    sh "docker tag two-tier-flask-app ${env.dockerHubUser}/two-tier-flask-app:latest"
                    sh "docker push ${env.dockerHubUser}/two-tier-flask-app:latest"
                }
            }
        }

        stage("Deploy") {
            steps {
                sh "docker compose up -d --build"
            }
        }
    }
    post{
        success{
            script{
                emailext from: 'altaftamboli2412@gmail.com',
                to: 'altaftamboli2412@gmail.com',
                body: 'Build success for Demo CICD App',
                subject: 'Build success for Demo CICD App'
            }
        }
        failure{
            script{
                emailext from: 'altaftamboli2412@gmail.com',
                to: 'altaftamboli2412@gmail.com',
                body: 'Build Failed for Demo CICD App',
                subject: 'Build Failed for Demo CICD App'
            }
        }
    }
}
  
      
      
  


