pipeline {
  agent {
    label 'worker_node2'
  }

    stages {
        stage('Checkout') {
            steps {
                // Get some code from a GitHub repository
                checkout([$class: 'GitSCM', branches: [[name: '*/main']], extensions: [], userRemoteConfigs: [[url: 'https://github.com/Astar-S/maven-project.git']]])
        
            }
        }  
      
        stage('Build') {
            steps {

                // Run Maven on a Unix agent.
                sh "mvn clean package"

            }
        }
      
        stage('Build Docker Image') {
            steps {

              sh "docker build -t webapp:v${BUILD_NUMBER} ." 

            }
        }
      
        stage('Push Image to Docker Hub') {
            steps {

              sh "docker tag webapp:v${BUILD_NUMBER} yuqis/webapp:v${BUILD_NUMBER}"
              sh "docker push yuqis/webapp:v${BUILD_NUMBER}"

            }
        }
      
        stage('Run the webapp container') {
            steps {

              sh "docker run --name webapp_v${BUILD_NUMBER} -p 8087:5000 -d yuqis/webapp:v${BUILD_NUMBER}"

            }
        }
    }
}
