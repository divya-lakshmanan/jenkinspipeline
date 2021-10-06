pipeline {
  
  environment {
  registryCredential = '2'
  }

  agent any

  stages {

    stage('Checkout Source') {
      steps {
        git url:'https://github.com/divya-lakshmanan/jenkinspipeline.git', branch:'main'
      }
    }
    
      stage("Build image") {
            steps {
                script {
                    myapp = docker.build("purplephoenix/hellowhale:${env.BUILD_ID}")
                }
            }
        }
    
      stage("Push image") {
            steps {
                script {
                  docker.withRegistry( '', registryCredential ) 
                  {
                    myapp.push("$BUILD_NUMBER")
                    myapp.push('latest')
                  }
                }
            }
        }

    
    stage('Deploy App') {
      steps {
        script {
          kubernetesDeploy(configs: "hellowhale.yml", kubeconfigId: "mykubeconfig")
        }
      }
    }

  }

}
