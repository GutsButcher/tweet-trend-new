pipeline {
    agent {
        node {
            label 'maven'
        }
    }
    environment {
      PATH= "/opt/apache-maven-3.9.9/bin:$PATH"
    }
    stages {
        stage("build"){
          steps{
            echo "------------BUILD STARTED-----------"
            sh 'mvn clean deploy -Dmaven.test.skip=true'
            echo "------------BUILD FINISHED-----------"
          }
        }
        stage("test"){
          steps{
        echo "------------UNTI TEST STARTED-----------"
        sh 'mvn surefire-report:report'
        echo "------------UNTI TEST FINISHED-----------"
      }
    }
  }
}
