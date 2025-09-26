pipeline{
    agent any

    tools {
        jdk 'JDK11'
        maven 'Maven'
    }

    stages{
        stage('Git checkout') {
            steps {
                    git branch: 'manoj-CI-project', url: 'https://github.com/suri7024/DevProjects.git'
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
        stage('Build Docker Image') {
            steps {
                    sh 'docker build -t suresh7024/project-1 .'
            }
        }

        stage('Docker image scan'){
            steps {
                    sh 'trivy image --format table -o trivy-image-report.html suresh7024/project-1'
            }
        }
        stage('Containersation'){
            steps {
                    sh '''
                    docker stop c1 || true
                    docker rm c1 || true
                    docker run -it -d --name c1 -p 9000:8080 suresh7024/project-1
                    '''
            }
        }
        stage('Login to Docker Hub'){
            steps {
                script {
                    withCredentials([usernamePassword(credentialsId: 'dockerhub-credentials', passwordVariable: 'DOCKER_PASSWORD', usernameVariable: 'DOCKER_USERNAME')]) {
                        sh 'echo $DOCKER_PASSWORD | docker login -u $DOCKER_USERNAME --password-stdin'
                    }
                }
            }
        }

        stage('Push Docker Image to Docker Hub') {
            steps {
                sh 'docker push suresh7024/project-1'
            }
            
        }

    }
}
