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
        CORS_ALLOWED_ORIGINS=*
    }

    stages {
        stage('Checkout') {
            steps {
                git 'https://github.com/kimyp0209/jenkins-ccom.git'
            }
        }
        stage('Build') {
            steps {
                bat './gradlew clean build -x test'
            }
        }
 
        stage('Run') {
            steps {
                bat 'java -jar build\\libs\\app1-0.0.1-SNAPSHOT.jar'
            }
        }
         stage('Deploy to AWS EC2') {
            steps {
                withCredentials([string(credentialsId: 'OPEN_API_KEY', variable: 'OPEN_API_KEY')]) {
                    script {
                        writeFile file: '.env', text: """
DB_HOST=localhost
DB_NAME=myapp
DB_USER=jenkins
DB_PASS=jenkins123
UPLOAD_PATH=/home/ec2-user/uploads
TOSS_CLIENT_KEY=test_ck_E92LAa5PVbbbmKAkDZmJV7YmpXyJ
TOSS_SECRET_KEY=test_sk_Z1aOwX7K8mzzkwkOkq2W3yQxzvNP
OPEN_API_KEY=${env.OPEN_API_KEY}
"""
                    }
                    bat """
                          echo Step 2: Send .env to EC2
    C:/Users/M/.ssh/pscp.exe -i C:/Users/M/.ssh/tim.ppk -batch -hostkey "ssh-ed25519 255 SHA256:h6fF/KbgIbLrQ4ZjcaJRccjQhrBmBZPu7n3M8VCSEZE" .env ec2-user@ec2-52-79-237-175.ap-northeast-2.compute.amazonaws.com:/home/ec2-user/

    echo Step 3: Send JAR to EC2
    C:/Users/M/.ssh/pscp.exe -i C:/Users/M/.ssh/tim.ppk -batch -hostkey "ssh-ed25519 255 SHA256:h6fF/KbgIbLrQ4ZjcaJRccjQhrBmBZPu7n3M8VCSEZE" backend/build/libs/app1-0.0.1-SNAPSHOT.jar ec2-user@ec2-52-79-237-175.ap-northeast-2.compute.amazonaws.com:/home/ec2-user/backend/build/libs/

    echo Step 4: Restart app on EC2
    C:/Users/M/.ssh/plink.exe -i C:/Users/M/.ssh/tim.ppk -batch -hostkey "ssh-ed25519 255 SHA256:h6fF/KbgIbLrQ4ZjcaJRccjQhrBmBZPu7n3M8VCSEZE" ec2-user@ec2-52-79-237-175.ap-northeast-2.compute.amazonaws.com ^
    "pkill -f app1-0.0.1-SNAPSHOT.jar || true; set -a; source /home/ec2-user/.env; set +a; nohup java -jar /home/ec2-user/backend/build/libs/app1-0.0.1-SNAPSHOT.jar > app.log 2>&1 &"
    exit 0
                    """
                }
            }
        }
    }
}
