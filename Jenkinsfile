def new_master = false;
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
        stage('new master') {
           when {
              allOf {
                changeset 'bundles/**'
              }
              beforeAgent true
            } 
            steps {
                container('maven'){
                    echo "${COMMIT_FILES}"
                        //echo "${IMG_NAME}"
                    script { new_master = true }
                }
            }
        }
        stage('change to master') {
           when {
               allOf {
                //changeset 'bundles/*/*'
                expression { new_master == false } 
               }
             
              beforeAgent true
            } 
            steps {
                container('maven'){
                        echo "${COMMIT_FILES}"
                        //echo "${IMG_NAME}"
                }
            }
        } 
        
    }
}
      
