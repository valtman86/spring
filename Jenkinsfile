pipeline {
    agent {
        docker {
            image 'maven:3.8.1-adoptopenjdk-11' 
            args '-v /root/.m2:/root/.m2' 
        }
    }
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -f demo/pom.xml -B -DskipTests clean package' 
            }
        }
         stage('Test') {
            steps {
                sh 'mvn -f demo/pom.xml test'
            }
            post {
                always {
                    sh 'docker create --name temporary-container spring-image'
                    sh 'docker cp temporary-container:/var/www/java/target/spring-reports .'
                    sh 'docker rm temporary-container'
                    junit 'spring-reports'
                }
            }
        }
    }
}
