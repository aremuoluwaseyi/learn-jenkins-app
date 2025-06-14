pipeline {
    agent any

    environment {
        NETLIFY_SITE_ID = 'de4f2189-690f-4359-85d3-1c411e5d9eaf'
    }

    stages {
        stage('Build') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
           
            steps{
                sh '''
                ls  -la
                node --version
                npm --version
                npm ci
                npm run build
                ls -la

                '''
            }
        }
        stage('Test'){
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
            steps{
                sh '''
                test -f build/index.html
                npm test
                '''
            }
        }
        stage('Deploy') {
            agent{
                docker{
                    image 'node:18-alpine'
                    reuseNode true
                }
            }
           
            steps{
                sh '''
                npm install netlify-cli
                node_modules/.bin/netlify --version
                echo 'Deploy to production netlify ID : de4f2189-690f-4359-85d3-1c411e5d9eaf'
                '''
            }
        }    
    }
    post {
        always{
            junit 'test-results/junit.xml'
        }
    }
}