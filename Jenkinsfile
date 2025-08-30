pipeline {
    agent any

    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages{
        stage('Git checkout') {
            steps {
                git branch: 'main', url: 'https://github.com/yashb1117/Dockersys.git'
            }
        }

        stage('Compile') {
            steps {
                    sh 'mvn compile'
            }         
        }
        stage('Build'){
            steps {
                    sh 'mvn clean install'
            }
        }

        stage('Docker Build') {
            steps {
                    sh 'docker build -t manojkrishnappa/project:1 .'
            }
        }

        stage('Docker image scan'){
            steps {
                    sh 'trivy image --format table -o trivy-image-report.html manojkrishnappa/project:1'
            }
        }
        stage('Containersation'){
            steps {
                    sh '''
                    docker stop c1 || true
                    docker rm c1 || true
                    docker run -it -d --name c1 -p 9000:8080 manojkrishnappa/project:1
                    '''
            }
        }
        stage('Login to Docker Hub'){
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
                sh 'docker push manojkrishnappa/project:1'
            }
        }
    }
}
