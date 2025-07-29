pipeline {
    agent any

    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages {
        stage('Git Checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashb1117/Dockersys.git'
            }
        }

        stage('Compile') {
            steps {
                sh 'mvn clean package'
            }
        }

        stage('Docker Build') {
            steps {
                sh 'docker build -t yashb1117/yash:1 .'
            }
        }

        stage('Docker Run (optional test)') {
            steps {
                 docker stop c1 || true
                docker rm c1 || true
                docker run -it -d --name c1 -p 9000:8080 yashb1117/project:1

            }
        }

        stage('Docker Login') {
            steps {
                script {
                    withCredentials([usernamePassword(
                        credentialsId: 'dockerhub-credentials',
                        usernameVariable: 'DOCKERHUB_USERNAME',
                        passwordVariable: 'DOCKERHUB_PASSWORD'
                    )]) {
                        sh 'echo $DOCKERHUB_PASSWORD | docker login -u $DOCKERHUB_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Docker Push') {
            steps {
                sh 'docker push yashb1117/yash:1'
            }
        }
    }
}
