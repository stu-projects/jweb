pipeline {
    agent {
        kubernetes {
            label 'maven_build'
            defaultContainer 'jnlp'
            yaml """
apiVersion: v1
kind: Pod
metadata:
  labels:
    some-label: some-label-value
spec:
  containers:
  - name: maven
    image: maven:alpine
    command:
    - cat
    tty: true
"""
        }
    }
    stages {
        stage('Run maven build') {
            steps {
                container('maven'){
                    configFileProvider([configFile(fileId: 'stusettingsxml', variable: 'MAVEN_SETTINGS_XML')]) {
                        sh "mvn -X -Dmaven.wagon.http.ssl.insecure=true -s $MAVEN_SETTINGS_XML package"
                    }
                }
            }
        }
    }
}
