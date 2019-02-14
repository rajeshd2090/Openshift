def checout(){
      checkout([$class: 'GitSCM', branches: [[name: '*/master']], doGenerateSubmoduleConfigurations: false, extensions: [], submoduleCfg: [], userRemoteConfigs: [[url: "${GIT_URL}"]]])
}
pipeline {
    agent any
    environment {
        GIT_URL='https://github.com/abhinav-goyall/os-mul-test-dum/tree/master/sales-service'
    }
    tools{
        maven 'Maven_HOME'
        jdk   'JAVA_HOME'
    }
 stages {
        stage ('testconvert - Checkout') {
            steps{
                
                checout()
            }
 	  
        }
        stage('Test') {
            steps {
                bat 'mvn test'
                junit '**/target/surefire-reports/*.xml'
            }
        }
        stage('Packaging') {
            steps {
                bat 'mvn war:war' 
            }
        }
        stage('Deploy') {
            steps {
                bat 'mvn deploy' 
            }
        }
        
}
post {
        always {
            echo "I AM ALWAYS first"
        }
        aborted {
            echo "BUILD ABORTED"
        }
        success {
            echo "BUILD SUCCESS"
        }
        unstable {
            echo "BUILD UNSTABLE"
        }
        failure {
            echo "BUILD FAILURE"
        }
    }
}
