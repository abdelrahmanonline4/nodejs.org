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
                    // يفضل استخدام 'npm ci' لتثبيت التبعيات بناءً على package-lock.json
                    sh 'npm ci'
                }
            }
        }
        stage('Run Tests') {
            steps {
                dir('nodejs.org') {
                    sh 'npm test'
                }
            }
        }
        stage('Build Docker Image') {
            steps {
                dir('nodejs.org') {
                    sh 'docker build -t bedomm180/nodejs.org .'  // تحديث اسم المستخدم واسم الصورة للدوكر
                }
            }
        }
        stage('Push Docker Image') {
            steps {
                dir('nodejs.org') {
                    // استخدام بيانات الاعتماد لدوكر هب
                    withCredentials([usernamePassword(credentialsId: 'docker-hub-credentials', usernameVariable: 'DOCKER_USERNAME', passwordVariable: 'DOCKER_PASSWORD')]) {
                        sh 'echo "$DOCKER_PASSWORD" | docker login -u "$DOCKER_USERNAME" --password-stdin'
                        sh 'docker push bedomm180/nodejs.org'  // تأكد من صحة اسم الصورة
                    }
                }
            }
        }
    }
}
