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
        string(defaultValue: "foo", description: 'App Name ?', name: 'p1')
        string(defaultValue: "bar", description: 'Component Name ?', name: 'p2')
        
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
        stage('call cd proceedure'){    
            steps{
               step([$class: 'ElectricFlowRunProcedure',
                      configuration: 'CdConfiguration',
                      projectName : 'nectar',
                      procedureName : 'chkCreds',
                      procedureParameters : """{"procedure":{"procedureName":"chkCreds",
                      "parameters":[
                            {"actualParameterName":"p1","value":"${params.p1}"},
                            {"actualParameterName":"p2","value":"${params.p2}"}
                      ]}}"""
                ])
            }
        }
        //stage('new call'){
        //    steps{
        //        cloudBeesFlowCallRestApi body: '', configuration: 'CdConfiguration', envVarNameForResult: 'CALL_REST_API_CREATE_PROJECT_RESULT', httpMethod: 'POST', parameters: [[key: 'projectName', value: 'EC-TEST-Jenkins-1.00.00.02'], [key: 'description', value: 'Native Jenkins Test Project']], urlPath: '/projects'
        //       
        //    }
        //}

        //stage('Call a cd procedure'){
        //    steps{
        //        //cloudBeesFlowRunProcedure configuration: 'CdConfiguration', overrideCredential: [credentialId: 'CREDS_PARAM'], procedureName: 'TomcatCheckServer', procedureParameters: '{"procedure":{"procedureName":"TomcatCheckServer","parameters":[{"actualParameterName":"max_time","value":"10"},{"actualParameterName":"tomcat_config_name","value":"Tomcat configuration"}]}}', projectName: 'CloudBees'
        //        cloudBeesFlowRunProcedure configuration: 'CdConfiguration', procedureName: 'chkCreds', procedureParameters: '{"procedure":{"procedureName":"chkCreds","parameters":[{"actualParameterName":"p1","value":"${params.p1}"},{"actualParameterName":"p2","value":"${params.p2}"}]}}', projectName: 'Honey'
        //
        //
        //    }
        //}
        
        
        stage('Publish an artifact to CD'){
            steps{
                cloudBeesFlowPublishArtifact configuration: 'CdConfiguration',
                                             repositoryName: 'default',
                                             artifactName: 'com.stushq:jweb',
                                             artifactVersion: "${env.BUILD_NUMBER}" ,filePath: 'target/jweb.war'
            }
        }
      
        stage('Deploy Application'){
            steps{
               /*cloudBeesFlowDeployApplication applicationName: 'honey',
                                              configuration: 'CdConfiguration',
                                              applicationProcessName: 'InstallHoney',
                                              environmentName: 'dev', projectName: 'nectar'
                */
                cloudBeesFlowDeployApplication applicationName: 'honey',
                                               applicationProcessName: 'InstallHoney',
                                               configuration: 'CdConfiguration',
                                               deployParameters: '{"runProcess":{"applicationName":"honey","applicationProcessName":"InstallHoney","parameter":[' +
                                                       '{"actualParameterName":"Parameter1","value":${env.BUILD_NUMBER}},' +
                                                       '{"actualParameterName":"Parameter2","value":"value2"}' +
                                                       ']}}',
                                               environmentName: 'dev',
                                               projectName: 'nectar'

            }
        }
            
    }
}
