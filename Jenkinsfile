pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                echo 'Running build automation'
                sh './gradlew build --no-daemon'
                archiveArtifacts artifacts: 'dist/trainSchedule.zip'
            }
        }
        
         stage('Build Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    app = docker.build("sindhudockerhub/train-schedule")
                    echo 'build completed'
                    app.inside {
                        echo 'inside start'
                        sh 'echo $(curl localhost:8080)'
                        echo 'inside end'
                    }
                }
            }
           }
      stage('Push Docker Image') {
            when {
                branch 'master'
            }
            steps {
                script {
                    docker.withRegistry('https://registry.hub.docker.com', 'jenkins_docker_credentails') {
                        app.push("${env.BUILD_NUMBER}")
                        app.push("latest")
                    }
                }
            }
        }  
    }
 }
