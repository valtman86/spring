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
                    junit allowEmptyResults: true, testResults:"${WORKSPACE}/test-results/*.xml"
                }
            }
        }
        stage('Deliver') { 
            steps {
                //sh "chmod +x -R ${env.WORKSPACE}"
                //sh './deliver.sh' 
                checkout scm
                //sh 'env && ./demo/mvnw effective-settings'
                //sh 'mvnw -f demo/mvnw -B -DskipTests clean package'
                script {
                
                   docker.build("loadrunner:${env.BUILD_ID}") 
                }
                //docker.build("myorg/myapp").push()
                //def customImage = docker.build("my-image:${env.BUILD_ID}")
                //customImage.inside {
                //    sh './demo/mvnw -B -DskipTests clean package'
                // }
            }
        }
        
    }
}
