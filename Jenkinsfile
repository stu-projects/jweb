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
                    configFileProvider([configFile(fileId: 'settingxml', variable: 'MAVEN_SETTINGS_XML')]) {
                        sh "mvn -X -s $MAVEN_SETTINGS_XML deploy"
                    }
                }
            }
        }
    }
}
