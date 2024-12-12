pipeline {
    agent {
        kubernetes {
            yaml '''
                apiVersion: v1
                kind: Pod
                spec:
                  containers:
                    - name: maven
                      image: maven:3.9.5-eclipse-temurin-17-alpine
                      env:
                        - name: HTTP_PROXY
                          value: http://your-proxy-url:port
                        - name: HTTPS_PROXY
                          value: http://your-proxy-url:port
                        - name: NO_PROXY
                          value: "localhost,127.0.0.1,.example.com"
                      command:
                        - sleep
                      args:
                        - infinity
                 '''
            defaultContainer 'maven'
        }
    }
  environment {
        MAVEN_OPTS="-Dhttp.proxyHost=proxy.example.com -Dhttp.proxyPort=8080 -Dhttps.proxyHost=proxy.example.com -Dhttps.proxyPort=8080 -Dhttp.nonProxyHosts=localhost|127.0.0.1|*.example.com"
    }  
    stages {
        stage('Build') {       
            steps {
                sh "env |sort"
               // Run the maven build
                sh "mvn -Dmaven.repo.local=/tmp/m2 clean install"
                archiveArtifacts "target/*.war"
            }
        }
    }
}


