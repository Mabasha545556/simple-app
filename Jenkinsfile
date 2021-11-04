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
                    sh "ssh -o StrictHostKeyChecking=no centos@172.31.40.103"
                    script{
                        try{
                           sh "ssh centos@172.31.40.103 kubectl apply -f tomcat.yml"   
                        }catch (error){
                           sh "ssh centos@172.31.40.103 kubectl create -f tomcat.yml"
                        }
                     }
                   }
               }
            }
        }
    }
