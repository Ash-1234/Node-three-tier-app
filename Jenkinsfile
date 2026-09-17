pipeline {
    agent any

    stages {

        stage('Checkout') {
            steps {
                checkout([
                    $class: 'GitSCM',
                    branches: [[name: '*/node-app']],
                    doGenerateSubmoduleConfigurations: false,
                    extensions: [],
                    submoduleCfg: [],
                    userRemoteConfigs: [[
                        url: 'https://github.com/Heyyprakhar1/Node-three-tier-app.git'
                    ]]
                ])
            }
        }

        stage('Trivy Filesystem Scan') {
            steps {
                sh '''
                    trivy fs \
                    --severity HIGH,CRITICAL \
                    
                    .
                '''
            }
        }

        stage('Docker Build') {
            parallel {

                stage('Build Frontend') {
                    steps {
                        sh '''
                            docker build \
                            -t node-frontend:${BUILD_NUMBER} \
                            ./frontend
                        '''
                    }
                }

                stage('Build Backend') {
                    steps {
                        sh '''
                            docker build \
                            -t node-backend:${BUILD_NUMBER} \
                            ./backend
                        '''
                    }
                }
            }
        }

        stage('Trivy Image Scan') {
            parallel {

                stage('Scan Frontend') {
                    steps {
                        sh '''
                            trivy image \
                            --severity HIGH,CRITICAL \
                            --exit-code 1 \
                            node-frontend:${BUILD_NUMBER}
                        '''
                    }
                }

                stage('Scan Backend') {
                    steps {
                        sh '''
                            trivy image \
                            --severity HIGH,CRITICAL \
                            --exit-code 1 \
                            node-backend:${BUILD_NUMBER}
                        '''
                    }
                }
            }
        }
    }
}
