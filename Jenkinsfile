pipeline {
    agent any
    stages{
        stage ("Git Checkout"){
            steps{
                git 'https://github.com/Mabasha545556/simple-app.git'
            }
        }
        stage ("Maven Build"){
            steps{
            sh 'mvn clean install package'
            }
        } 
        stage ("Build Docker Image"){
            steps{
           sh 'docker build -t mabasha/myapp:tomcat . '
            }
        }
        stage ("Push Docker Image"){
            steps{
            withCredentials([string(credentialsId: 'docker', variable: 'dockerhubpwd')]) {
             sh "docker login -u mabasha -p ${dockerhubpwd}"
        }    
            sh 'docker push mabasha/myapp:tomcat'
            }
        }
          stage ('Deploy to K8s'){
            steps{
                sshagent(['kubernets']) {
                    sh "ssh -o StrictHostKeyChecking=no ubuntu@65.0.168.43 kubectl apply -f . "
                   }
               }
            }
        }
    }
