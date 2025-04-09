pipeline {
   agent any
   environment {
       DOCKER_IMAGE = 'vinayz7/java-web-app-cicd:latest'
   }
   stages {
       stage('Clone Repository') {
           steps {
               git url: 'https://github.com/vinayz7/cicd', branch: 'master'
           }
       }
     stage('build mvn for jar') {
           steps {
              sh 'mvn clean install'
           }
       }      
       
    stage('Build Docker Image') {
           steps {
               sh 'docker build -t $DOCKER_IMAGE .'
           }
       }
       stage('Push Docker Image') {
           steps {
               withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USER', passwordVariable: 'DOCKER_PASS')]) {

                   sh """
                       docker login -u $DOCKER_USER -p $DOCKER_PASS
                   """
                   sh 'docker push $DOCKER_IMAGE'
               }
           }
       }
       stage('Deploy to Kubernetes') {
           steps {
               sh 'kubectl apply -f deploy-tomcat.yaml'
           }
       }
       stage('Expose Service') {
           steps {
               sh 'kubectl apply -f service.yaml'
           }
       }
   }
}
