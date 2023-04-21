node {
    def app

    stage('Git Checkout') {
        checkout scm
    }
    stage('Unit testing') {
        sh 'mvn test'
    }
    stage('Maven Build') {
        sh 'mvn clean install'
    }
    stage('Elastic Container Registry Login'){
            
             sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/p5u5p5h0'     
    }

    stage('Docker Image Build') {
  
       app = docker.build("public.ecr.aws/p5u5p5h0/buchananecr:${env.BUILD_NUMBER}")
    }
    stage('Push image to ECR Repository') {
            app.push("${env.BUILD_NUMBER}")
    }
    stage('Trigger Update Manifest') {
        echo "triggering Update manifest Job"
            build job: 'FinalCD', parameters: [string(name: 'DOCKERTAG', value: env.BUILD_NUMBER)]
    }
    
}