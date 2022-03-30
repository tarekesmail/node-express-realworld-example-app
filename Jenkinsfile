pipeline {
  agent any
    stages {
      stage('Build') {
          steps {
            script {
                docker.build("my-image:${env.BUILD_ID}")
            }
       }
    }
    stage('Run Tests') {
      steps {
        script {
          docker.image('mongo:3.4').withRun('--net-alias=mongo') {c ->
            docker.image("my-image:${env.BUILD_ID}").withRun("--name nodeapp-${env.BUILD_ID} -e MONGODB_URI='mongodb://db/conduit' --link ${c.id}:db") {
                sh "docker exec nodeapp-${env.BUILD_ID} npm test"
            }
          }
        }
      }
    }
  }
}
