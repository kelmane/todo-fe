pipeline {
    agent any
    stages {
        stage('Build stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t image-builder --target builder .'
            }
        }
        stage('Test stage') {
            steps {
              sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t image-testing --target testing .'
            }
        }
        stage('Delivery stage') {
            steps {
                sh 'DOCKER_BUILDKIT=1 docker build -f Dockerfile-pipelines -t nissimacheroff/todo-fe:jenkins-$BUILD_NUMBER --target delivery .'
            }
        }
        stage('Cleanup') {
            steps {
                echo "Cleanup-stage"
                sh 'docker system prune -f'
            }
        }
        stage('push') {
            environment {
                dockerpwd = credentials('docker_pwd')
            }
            steps {
                    sh 'docker login -u kelmane -p ${dockerpwd}'
                    sh "docker push kelmane/todo-fe:jenkins-$BUILD_NUMBER"
        }
    }
}
}
