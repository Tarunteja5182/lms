pipeline {
    agent any
    environment {
        // More detail: 
        // https://jenkins.io/doc/book/pipeline/jenkinsfile/#usernames-and-passwords
        NEXUS_CRED = credentials('nexus')
   }

    stages {
        stage('Build') {
            steps {
                echo 'Building..'
                sh 'cd webapp && npm install && npm run build'
            }
        }
        stage('Test') {
            steps {
                echo 'Testing..'
                sh 'cd webapp && sudo docker container run --rm -e SONAR_HOST_URL="http://3.143.232.93:9000/" -e SONAR_LOGIN="sqp_114c6b49febca15d6cb226477d84b9b1cda02bc4" -v ".:/usr/src" sonarsource/sonar-scanner-cli -Dsonar.projectKey=lms'
            }
        }
        stage('Release') {
            steps {
                echo 'Release Nexus'
                sh 'rm -rf *.zip'
                sh 'cd webapp && zip dist-${BUILD_NUMBER}.zip -r dist'
                sh 'cd webapp && curl -v -u $Username:$Password --upload-file dist-${BUILD_NUMBER}.zip http://20.172.187.108:8081/repository/lms/'
            }
        }
    }
}
