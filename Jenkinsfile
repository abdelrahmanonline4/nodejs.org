pipeline {
    agent any

    stages {
        stage('Clone Repository') {
            steps {
                script {
                    if (!fileExists('nodejs.org')) {
                        sh 'git clone https://github.com/abdelrahmanonline4/nodejs.org'
                    } else {
                        dir('nodejs.org') {
                            sh 'git fetch'
                            sh 'git checkout Master'
                            sh 'git pull'
                        }
                    }
                }
            }
        }
        stage('Install Dependencies') {
            steps {
                dir('nodejs.org') {
                    // استخدم المسار لـ node و npm مباشرةً
                    sh 'export PATH="/home/qmo/.nvm/versions/node/v18.20.4/bin:$PATH" && npm install'
                }
            }
        }
        stage('Run Tests') {
            steps {
                dir('nodejs.org') {
                    sh 'export PATH="/home/qmo/.nvm/versions/node/v18.20.4/bin:$PATH" && npm test'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                dir('nodejs.org') {
                    sh 'docker build -t bedomm180/nodejs.org .'
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                dir('nodejs.org') {
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                        sh 'docker push bedomm180/nodejs.org'
                    }
                }
            }
        }
    }

    post {
        success {
            echo 'Build, test, and push completed successfully!'
        }
        failure {
            echo 'Build, test, or push failed!'
        }
    }
}

