pipeline {
    agent any
    options {
        timeout(time: 20, unit: 'MINUTES')
    }
    stages{
        // NPM dependencies
        stage('pull npm dependencies') {
            steps {
                sh 'npm install'
            }
        }
       stage('build Docker Image') {
            steps {
                script {
                    // build image
                    docker.build("346798024921.dkr.ecr.us-east-1.amazonaws.com/netflix:latest")
               }
            }
        }
        stage('Trivy Scan (Aqua)') {
            steps {
                sh 'trivy image --format template --output trivy_report.html 346798024921.dkr.ecr.us-east-1.amazonaws.com/netflix:latest'
            }
       }
        stage('Push to ECR') {
            steps {
                script{
                    //https://<AwsAccountNumber>.dkr.ecr.<region>.amazonaws.com/netflix', 'ecr:<region>:<credentialsId>
                    docker.withRegistry('https://346798024921.dkr.ecr.us-east-1.amazonaws.com/netflix', 'ecr:us-west-1:easante817') {
                    // build image
                    def myImage = docker.build("346798024921.dkr.ecr.us-east-1.amazonaws.com/netflix:latest")
                    // push image
                    myImage.push()
                    }
                }
            }
        }
        
    }
}
