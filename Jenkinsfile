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
                    sh "scp -o StrictHostKeyChecking=no services.yml pods.yml ec2-user@65.0.104.80:/home/ec2-user/"
                   script{
                       try{
                           sh "ssh ec2-user@65.0.104.80 kubectl apply -f . "
                       }catch (error){
                           sh "ssh ec2-user@65.0.104.80 kubectl create -f . "
                       }
                   }
               }
            }
        }
    }
}
