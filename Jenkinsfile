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
    
    environment {
        COMMIT_FILES = sh(script: 'git show --pretty="" --name-only', , returnStdout: true).trim()
        
    }

    stages {
        stage('Run maven build') {
            steps {
                container('maven'){
                   
                      
                        echo "${COMMIT_FILES}"
                        sh '''
                           echo ${COMMIT_FILES} | awk -F/ '{print $2}'    
                        '''
                    
                    
                }
            }
        }
    }
}
