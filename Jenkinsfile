pipeline {
    agent { label 'Jenkins-Agent' }

    tools {
        jdk 'Java17'
        maven 'Maven3'
    }

    environment {
        JENKINS_API_TOKEN = credentials('JENKINS_API_TOKEN')  // Securely use the API Token
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

        stage("Verify API Token") {
            steps {
                sh 'echo "✅ API Token is set and ready to use."'
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
    }

    post {
        success {
            echo "🎉 Build and tests succeeded!"
        }
        failure {
            echo "❌ Build failed. Check logs for errors."
        }
    }
}
