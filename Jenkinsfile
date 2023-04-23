pipeline {
    agent any

     environment{
       registryCredential = 'ecr:.ap-northeast-1:Pittu'
       appRegistry = "<857459973995.dkr.ecr.ap-northeast-1.amazonaws.com/202052330_cluster_project>"
       capstoneRegistry = "<https://857459973995.dkr.ecr.ap-northeast-1.amazonaws.com>"
       cluster = "<202052330_cluster_project>"
        service = "<202052330_capstone>"
   }

    stages {
        stage('Clone Website') {
            steps {
                git url:''
            }
        }

       stage("Docker Build Image"){
           steps{
             script{
                dockerImage = docker.build(appRegistry+ ":$BUILD_NUMBER",".")
             }
           }
      }

       stage('Test Website') {
            steps {
                // Run tests on the website
                sh 'echo "Running tests on the website"'
            }
       }

       stage("Upload App Image"){
         steps{
            script{
                docker.withRegistry(capstoneRegistry, registryCredential){
                    dockerImage.push("$BUILD_NUMBER")
                    dockerImage.push('latest')
                }
            }
         }
      }

      stage('Deploy to ecs'){
         when {
                branch "master"
         }
         steps{
            withAWS(credentials: '<CredentialID>', region: 'us-east-1'){
                sh 'aws ecs update-service --cluster ${cluster} --service ${service} --force-new-deployment'
            }
         }
      }

    }
}
