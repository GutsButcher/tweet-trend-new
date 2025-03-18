       def registry = 'https://trialbn90ue.jfrog.io'
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
       stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"jfrog-token"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "libs-release-local/{1}",
                              "flat": "false",
                              "props" : "${properties}",
                              "exclusions": [ "*.sha1", "*.md5"]
                            }
                         ]
                     }"""
                     def buildInfo = server.upload(uploadSpec)
                     buildInfo.env.collect()
                     server.publishBuildInfo(buildInfo)
                     echo '<--------------- Jar Publish Ended --------------->'  
            
            }
        }   
    }
    }
  }
}
