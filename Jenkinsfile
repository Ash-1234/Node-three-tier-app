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
                        url: 'https://github.com/Ash-1234/Node-three-tier-app.git'
                    ]]
                ])
            }
        }

        stage('Install Dependencies') {
            parallel {

                stage('Frontend Dependencies') {
                    steps {
                        dir('frontend') {
                            sh '''
                                npm ci
                            '''
                        }
                    }
                }

                stage('Backend Dependencies') {
                    steps {
                        dir('backend') {
                            sh '''
                                npm ci
                            '''
                        }
                    }
                }
            }
        }

        stage('Unit Testing') {
            parallel {

                stage('Unit Test - Frontend') {
                    steps {
                        dir('frontend') {
                            sh '''
                                npm test -- --watchAll=false
                            '''
                        }
                    }
                }

                stage('Unit Test - Backend') {
                    steps {
                        dir('backend') {
                            sh '''
                                npm test -- --watchAll=false
                            '''
                        }
                    }
                }
            }
        }

        stage('Trivy Filesystem Scan') {
    steps {
        sh '''
            trivy fs --severity HIGH,CRITICAL .
        '''
    }
}
    }
}
