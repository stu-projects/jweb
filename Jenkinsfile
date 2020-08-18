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
       // IMG_NAME = sh(script: 'echo -n '${COMMIT_FILES}' |cut -d/ -f1 ', , returnStdout: true).trim()
       
        
    }

    stages {
        stage('Run maven build') {
            steps {
                container('maven'){
                   
                      
                        echo "${COMMIT_FILES}"
                        //echo "${IMG_NAME}"
                        
                    
                    
                }
            }
        }
    }
    stage ("Deploy branches") {
    agent any

    when { 
        allOf {
            not { branch 'master' }
            changeset "bundles/**"
            expression {  // there are changes in some-directory/...
                sh(returnStatus: true, script: 'git diff  origin/master --name-only | grep --quiet "^bundles/.*"') == 0
            }
            expression {   // ...and nowhere else.
                sh(returnStatus: true, script: 'git diff origin/master --name-only | grep --quiet --invert-match "^some-directory/.*"') == 1
            }
        }
    }

    steps {
       echo "running"
    }
}
}
