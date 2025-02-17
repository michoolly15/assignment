pipeline {
    agent any

    environment {
        NODE_VERSION = '23.8.0' 
    }

    stages {
        stage(' My_Checkout_Code') {
            steps {
                git branch: 'main', url: 'https://github.com/michoolly15/assignment.git'
            }
        }

        stage('Detect Changes') {
            steps {
                script {
                    def mike_assignment = sh(script: 'git diff --name-only HEAD~1', returnStdout: true).trim()
                    
                    env.BUILD_ANGULAR = mike_assignment.contains('angular-app/') ? 'true' : 'false'
                    env.BUILD_REACT   = mike_assignment.contains('react-app/') ? 'true' : 'false'
                    env.BUILD_VUE     = mike_assignment.contains('vue-app/') ? 'true' : 'false'
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
                        sh 'rm -r dist/*'

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
                        sh 'rm -r dist/*'

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
                        sh 'rm -r dist/*'

                    }
                }
            }
        }
    }
}

