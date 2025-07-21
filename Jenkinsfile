pipeline {
    agent any

    tools {
        jdk 'jdk17'
        gradle 'gradle8.7'
    }

    environment {
        DB_URL= 'jdbc:mysql://mysqltest-container:3306/cpgndb?characterEncoding=UTF-8'
        DB_USER = 'admin'
        DB_PASSWORD = '1234'
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
