// Jenkinsfile התחלתי — תרגיל 5
// המשימה: בקשו מ-AI להמיר את ה-workflow של GitHub Actions שבניתן ל-Jenkinsfile,
// והשלימו את השלבים החסרים (Lint, Test + פרסום תוצאות JUnit, הודעת סטטוס).
pipeline {
    agent any

    options {
        timestamps()
        buildDiscarder(logRotator(numToKeepStr: '10'))
    }

    stages {
        stage('Install') {
            steps {
                sh 'node --version && npm --version'
                sh 'npm ci'
            }
        }

        stage('Lint') {
            steps {
                sh 'npm run lint'
            }
        }

        stage('Test') {
            steps {
                sh 'npm run test:ci'
            }
            post {
                always {
                    junit 'reports/junit.xml'
                }
            }
        }
    }

    post {
        success {
            echo "Build #${env.BUILD_NUMBER} passed successfully"
        }
        failure {
            echo "Build #${env.BUILD_NUMBER} failed — ${env.BUILD_URL}console"
        }
        always {
            cleanWs(deleteDirs: true, notFailBuild: true)
        }
    }
}
