pipeline 
{
  agent any

  stages{
    stage(checkout){
        steps{
            checkout([$class: 'GitSCM',
                          branches: [[name: '*/main']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: https://github.com/Heyyprakhar1/Node-three-tier-app.git']]])
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
