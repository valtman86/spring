pipeline {
    agent any
    stages {
        stage('Build') { 
            steps {
                sh 'mvn -f demo/pom.xml -B -DskipTests clean package'
                sh "docker build -t loadrunner:${env.BUILD_ID} -f ./demo/Dockerfile ." 
            }
        }
        stage('Test') {
            steps {
                sh 'mvn -f demo/pom.xml test'
                sh "docker run -d -p 8081:8080 --name loadrunner loadrunner:${env.BUILD_ID} -v "
                sh "curl -v http://0.0.0.0:8080/"
                sh "docker stop loadrunner"
                sh "docker rm loadrunner"
                //sh 'docker ps -q -f status=exited | xargs --no-run-if-empty docker rm'
                
                }
            //}
            //post {
            //    always {
            //        junit allowEmptyResults: true, testResults:"${WORKSPACE}/test-results/*.xml"
            //    }
            //}
        }
                
    }
}
