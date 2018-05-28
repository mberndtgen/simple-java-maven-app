pipeline {
    agent any
    stages {
        stage('Build') {
            steps {
                sh 'mvn -B -DskipTests clean package'
            }
        }
        stage('Test') {
            steps {
                sh 'mvn test'
            }
            post {
                always {
                    junit 'target/surefire-reports/*.xml'
                }
            }
        }
        stage('Deliver') {
            steps {
                sh './jenkins/scripts/deliver.sh'
            }
        }
        stage('SonarQube analysis') {
            steps {
              sh 'mvn org.sonarsource.scanner.maven:sonar-maven-plugin:3.2:sonar'
            }
        }
        // No need to occupy a node
        stage("Quality Gate") {
          steps {
            timeout(time: 1, unit: 'HOURS') {
              // Just in case something goes wrong, pipeline will be killed after a timeout
              waitForQualityGate abortPipeline: true
            }
          }
        }
    }
}
