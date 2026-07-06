pipeline {
   agent any
   stages {
       stage('Checkout') {
           steps {
               git url: 'https://github.com/Senossi/devops.git', branch: 'main'
           }
       }
       stage('Build Application') {
           steps {
               sh './mvnw clean package -DskipTests'
           }
       }
       stage('Build Docker Image') {
           steps {
               sh '''
               docker build -t spring-boot-spring-security-jwt-authentication-app:latest .
               '''
           }
       }
       stage('Deploy') {
           steps {
               sh '''
               docker network create spring-network || true
               docker rm -f spring-boot-app || true
               docker run -d \
                 --name spring-boot-app \
                 --network spring-network \
                 -p 8081:8080 \
                 spring-boot-spring-security-jwt-authentication-app:latest
               '''
           }
       }
   }
   post {
       success {
           echo 'Pipeline Success 🚀'
       }
       failure {
           echo 'Pipeline Failed ❌'
       }
   }
}
