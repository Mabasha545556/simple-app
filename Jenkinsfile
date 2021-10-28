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
                sshagent(['mykubernets']) {
                    sh "ssh -o StrictHostKeyChecking=no ec2-user@65.0.104.80 kubectl apply -f tomcat"
                   }
               }
            }
        }
    }
}
