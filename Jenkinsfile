pipeline 
{
  agent any

  stages{
    stage(checkout){
        steps{
            checkout([$class: 'GitSCM',
                          branches: [[name: '*/node-app']],
                          doGenerateSubmoduleConfigurations: false,
                          extensions: [],
                          submoduleCfg: [],
                          userRemoteConfigs: [[url: 'https://github.com/Ash-1234/Node-three-tier-app.git']]])
        }
    

    }
  

    stage(install dependencies){
        parallel{
            stage('frontend dependencies'){
                steps{
                    dir(frontend)
                      sh '''
                         npn ci
                         '''
                }
            }
            stage('backend dependencies'){
                steps{
                    dir(backend)
                      sh '''
                         npn ci
                         '''
                }
            }
        }
    }

    stage(unit testing){
        parallel{
            stage( unit test on frontend){
              steps{
                dir(frontend)
                   sh '''
                      npm test -- --watchAll=false
                      '''

              }
            }
            stage( unit test on backend){
              steps{
                dir(backend)
                   sh '''
                      npm test -- --watchAll=false
                      '''

              }
            }
    }

  }

}
