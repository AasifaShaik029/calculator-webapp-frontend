pipeline {
    environment {
        imagename = "web"
        jenkinsProject = 'calculator-webapp-frontend'
    }

    agent any

    stages {
        stage('Git Staging') {
            steps {
                checkout([$class: 'GitSCM', 
                          branches: [[name: '*/main']], 
                          extensions: [], 
                          userRemoteConfigs: [[credentialsId: 'github-cred', url: 'https://github.com/yashrpandit/calculator-webapp-frontend.git']]
                ])
            }
        }

        stage('Build Image') {
            steps {
                // Cleanup previous container and image if necessary
                sh 'docker stop ${imagename} || true'  // Ensure it doesn't fail if container is not running
                sh 'docker rm ${imagename} || true'    // Remove container if exists
                sh 'docker rmi ${imagename} || true'   // Remove image if exists

                // Build new Docker image
                sh 'docker image build -t ${imagename} .'
            }
        }

        stage('Run Image') {
            steps {
                // Run Docker container from the built image
                sh 'docker run -p 81:80 --restart=always --name ${imagename} -d ${imagename}'
            }
        }
    }

    post {
        always {
            // Optional: Clean up workspace or print diagnostic info
            cleanWs()
        }
    }
}
