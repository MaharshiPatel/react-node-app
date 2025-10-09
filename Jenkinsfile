pipeline {
    agent any
    tools {
        jfrog 'jfrog-cli'
        nodejs 'npm'
    }
    stages {
        stage('Clone') {
            steps {
                git branch: 'main', url: "https://github.com/MaharshiPatel/react-node-app.git"
            }
        }
        
        stage('Check Connection') {
            steps {
                jf 'rt ping'
                sh """
                # Example of uploading files to Artifactory
                pwd
                ls -la
                """
            }
        }

        stage('Build and Publish Artifact') {
            steps {
                // Configure npm project's repositories
                jf 'npm-config --repo-resolve alpha-npm-virtual --repo-deploy alpha-npm-virtual'

                // Install dependencies
                jf 'npm install'
                  
                sh """
                ls -la
                """

                // Pack and deploy the npm package
                jf 'npm publish'
            }
        }
        
        stage('Publish build info') {
            steps {
                jf 'rt bp'
            }
        }
        
        stage('Build Scan') {
            steps {
                jf 'bs'
            }
        }

    }
}
