pipeline {
    agent any

    environment {
        // 필요한 환경 변수를 정의할 수 있습니다.
        // EXAMPLE_VAR = 'example_value'
    }

    stages {
        stage('Checkout') {
            steps {
                // Git 저장소에서 코드를 가져옵니다.
                git url: 'https://github.com/shiverlog/msdocs-nodejs-mongodb-azure-sample-app.git', branch: 'main'
            }
        }
        
        stage('Build') {
            steps {
                // 빌드 명령어를 실행합니다. 예: Maven, Gradle, npm 등
                sh 'mvn clean install'
            }
        }
        
        stage('Test') {
            steps {
                // 테스트 명령어를 실행합니다.
                sh 'mvn test'
            }
            post {
                always {
                    // 테스트 결과를 저장합니다.
                    junit '**/target/surefire-reports/*.xml'
                }
            }
        }

        stage('Deploy') {
            when {
                // 배포 조건을 설정합니다. 예: 특정 브랜치, 특정 환경 등
                branch 'main'
            }
            steps {
                // 배포 스크립트를 실행합니다.
                sh 'scripts/deploy.sh'
            }
        }
    }

    post {
        always {
            // 빌드 후 항상 실행되는 작업을 정의합니다.
            echo 'Cleaning up...'
            cleanWs()
        }
        success {
            // 빌드 성공 시 실행되는 작업을 정의합니다.
            echo 'Build was successful!'
        }
        failure {
            // 빌드 실패 시 실행되는 작업을 정의합니다.
            mail to: 'team@example.com',
                 subject: "Failed Pipeline: ${currentBuild.fullDisplayName}",
                 body: "Something is wrong with ${env.JOB_NAME}."
        }
    }
}