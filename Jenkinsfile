pipeline {
  agent any
  
   tools {nodejs "node"}
    
  stages {
    stage("Clone code from GitHub") {
            steps {
                script {
                    checkout scmGit(branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[credentialsId: 'GIT_CRED', url: 'https://github.com/prasantaaws1/Deploy-NodeApp-to-AWS-EKS-using-Jenkins-Pipeline']])
                }
            }
        }
     
    stage('Node JS Build') {
      steps {
        sh 'npm install'
      }
    }
  
     stage('Build Node JS Docker Image') {
            steps {
                script {
                  sh 'docker build -t pkbhub/node-app-1.0 .'
                }
            }
        }

    stage('Deploy Docker Image to DockerHub') {
            steps {
                script {
                 withCredentials([string(credentialsId: 'DOCKER_CRED', variable: 'DOCKER_CRED')]) {
                    sh 'docker login -u pkbhub -p ${DOCKER_CRED}'
            }
            sh 'docker push pkbhub/node-app-1.0'
        }
            }   
        }

    stage('Deploying Node App to Kubernetes') {
      steps {
        script {
          sh ('aws eks update-kubeconfig --name sample --region ap-south-1')
          sh "kubectl get ns"
          sh "kubectl apply -f nodejsapp.yaml"
        }
      }
    }


    

  }
}
