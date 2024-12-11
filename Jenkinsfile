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
    stages {
        stage('Build') {
            steps {
                //see https://plugins.jenkins.io/pipeline-maven/
                withMaven(
                        // Use `$WORKSPACE/.repository` for local repository folder to avoid shared repositories
                        mavenLocalRepo: '.repository',
                        //mavenSettingsConfig: 'global-maven-settings'
                ) {
                    // Run the maven build
                    sh "mvn  clean install"
                }
                archiveArtifacts "target/*.war"
            }
        }
    }
}


