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
    parameters{
        //choice(choices: ['dev', 'prd', 'ist'], description: 'What environment ?', name: 'envtarget')
        string(defaultValue: "", description: 'App Name ?', name: 'p1')
        string(defaultValue: "", description: 'Component Name ?', name: 'p2')
        
    }
    stages {
        stage('Run maven build') {
            steps {
                container('maven'){
                    configFileProvider([configFile(fileId: 'stusettingsxml', variable: 'MAVEN_SETTINGS_XML')]) {
                        sh "mvn -s $MAVEN_SETTINGS_XML deploy"
                        //sh "mvn -X -Dmaven.wagon.http.ssl.insecure=true -s $MAVEN_SETTINGS_XML deploy"
                    }
                }
            }
        }
    }
    stage('call cd proceedure'){
            
            
            steps{
                step([$class: 'clludbeesFlowRunProcedure',
                      configuration: 'CdConfiguration',
                      projectName : 'Honey',
                      procedureName : 'chkCreds',
                      procedureParameters : """{"procedure":{"procedureName":"chkCreds",
                      "parameters":[
                            {"actualParameterName":"p1","value":"${params.appName}"},
                            {"actualParameterName":"p2","value":"${params.compName}"}
                      ]}}"""
                ])
            }
        }
}
