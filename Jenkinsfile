pipeline {
    agent any

    environment {
        NODE_VERSION = '23.8.0' 
    }

    stages {
        stage('Checkout Code') {
            steps {
                git branch: 'main', url: 'https://github.com/your-org/your-repo.git'
            }
        }

        stage('Detect Changes') {
            steps {
                script {
                    def changes = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
                    
                    env.BUILD_ANGULAR = changes.contains('angular-app/') ? 'true' : 'false'
                    env.BUILD_REACT   = changes.contains('react-app/') ? 'true' : 'false'
                    env.BUILD_VUE     = changes.contains('vue-app/') ? 'true' : 'false'
                }
            }
        }

        stage('Build and Deploy Angular') {
            when {
                environment name: 'BUILD_ANGULAR', value: 'true'
            }
            steps {
                script {
                    dir('angular-app') {
                        sh 'npm install'
                        sh 'npm run build'
                        sh 'scp -r dist/* user@server:/var/www/angular-app/'
                    }
                }
            }
        }

        stage('Build and Deploy React') {
            when {
                environment name: 'BUILD_REACT', value: 'true'
            }
            steps {
                script {
                    dir('react-app') {
                        sh 'npm install'
                        sh 'npm run build'
                        sh 'scp -r build/* user@server:/var/www/react-app/'
                    }
                }
            }
        }

        stage('Build and Deploy Vue') {
            when {
                environment name: 'BUILD_VUE', value: 'true'
            }
            steps {
                script {
                    dir('vue-app') {
                        sh 'npm install'
                        sh 'npm run build'
                        sh 'scp -r dist/* user@server:/var/www/vue-app/'
                    }
                }
            }
        }
    }
}

