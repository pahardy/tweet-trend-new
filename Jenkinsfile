def registry = 'https://phardy.jfrog.io'
pipeline {
    agent {
        node {
            label'maven'
        }
    }

environment {
  PATH = "/opt/apache-maven-3.9.6/bin:$PATH"
}
    stages {
      stage("build") {
        steps {
          sh 'mvn clean deploy'
        }
      }

      stage("Jar Publish") {
        steps {
            script {
                    echo '<--------------- Jar Publish Started --------------->'
                     def server = Artifactory.newServer url:registry+"/artifactory" ,  credentialsId:"834895cc-5fef-4b66-a7d2-23b73111a051"
                     def properties = "buildid=${env.BUILD_ID},commitid=${GIT_COMMIT}";
                     def uploadSpec = """{
                          "files": [
                            {
                              "pattern": "jarstaging/(*)",
                              "target": "phardy-libs-release-local/{1}",
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
