pipeline {
    agent { label 'Jenkins-Agent' }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    stages {
        stage("Cleanup Workspace") {
            steps {
                cleanWs()
            }
        }

        stage("Checkout from SCM") {
            steps {
                git branch: 'main', credentialsId: 'github', url: 'https://github.com/Chinaza17/Register-App.git'
            }
        }

        stage("Build Application") {
            steps {
                sh "mvn clean package"
            }
        }

        stage("Test Application") {
            steps {
                sh "mvn test"
            }
        }

        stage("Build & Push Docker Image") {
            steps {
                sh '''
                docker build -t my-app:latest .
                docker tag my-app:latest your-dockerhub-username/my-app:latest
                docker login -u your-dockerhub-username -p your-dockerhub-password
                docker push your-dockerhub-username/my-app:latest
                '''
            }
        }

        stage("Deploy Application") {
            steps {
                sh '''
                docker pull your-dockerhub-username/my-app:latest
                docker stop my-app-container || true
                docker rm my-app-container || true
                docker run -d --name my-app-container -p 8080:8080 your-dockerhub-username/my-app:latest
                '''
            }
        }
    }

    post {
        success {
            echo "🎉 Deployment successful!"
        }
        failure {
            echo "❌ Deployment failed. Check logs for errors."
        }
    }
}
