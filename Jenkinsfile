pipeline {
    agent any

    tools {
        jdk 'jdk17'
        gradle 'gradle8.7'
    }

    environment {
        DB_HOST = 'localhost'
        DB_NAME = 'myapp'
        DB_USER = 'jenkins'
        DB_PASS = 'jenkins123'
        UPLOAD_PATH = '/home/ec2-user/uploads' 
        MAIL_USER = 'flowercharan0307@gmail.com'
        MAIL_PASSWORD = 'oaov kxrh zfaz mynm'
        TOSS_CLIENT_KEY = 'test_ck_E92LAa5PVbbbmKAkDZmJV7YmpXyJ'
        TOSS_SECRET_KEY = 'test_sk_Z1aOwX7K8mzzkwkOkq2W3yQxzvNP'
        OPEN_API_KEY = credentials('OPEN_API_KEY')
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kimyp0209/jenkins-ccom.git'
            }
        }
        stage('Build') {
            steps {
                bat './gradlew clean build'
            }
        }
        stage('Test') {
            steps {
                bat './gradlew test'
            }
        }
        stage('Run') {
            steps {
                bat 'java -jar build\\libs\\app-0.0.1-SNAPSHOT.jar'
            }
        }
    }
}
