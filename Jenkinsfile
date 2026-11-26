pipeline {
    agent { label 'devops1-deddy' }

    stages {
        stage('Pull SCM') {
            steps {
                git branch: 'main', url: 'https://github.com/deddy98/simple-apps.git'
            }
        }
        
        stage('Build') {
            steps {
                sh'''
                cd apps
                npm install
                '''
            }
        }
        
        stage('Testing') {
            steps {
                sh'''
                cd apps
                npm run test:coverage
                '''
            }
        }
        
        stage('Code Review') {
            steps {
                sh'''
                cd apps
                sonar-scanner \
                    -Dsonar.projectKey=simple-apps \
                    -Dsonar.sources=. \
                    -Dsonar.host.url=http://172.23.8.152:9000 \
                    -Dsonar.login=sqp_d5f4a5e1556e9c9bf8db11dc021f4405bc17669b
                '''
            }
        }
        
        stage('Deploy') {
            steps {
                sh'''
                docker compose up --build -d
                '''
            }
        }
        
        stage('Backup') {
            steps {
                 sh 'docker compose push' 
            }
        }
    }
}