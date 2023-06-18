def COLOR_MAP = [
    'SUCCESS': 'good',  // good in slack means green color
    'FAILURE': 'danger', // danger=red color
]
pipeline {
    agent any
    tools {
        maven "MAVEN3"   //same name we add in Jenkins
        jdk "OracleJDK8"
    }

    environment {
        SNAP_REPO = 'vprofile-snapshot'
        NEXUS_USER = 'admin'
        NEXUS_PASS = 'admin'
        RELEASE_REPO = 'vprofile-release'
        CENTRAL_REPO = 'vpro-maven-central'
        NEXUSIP = '172.31.20.107'    // may change , Private ip
        NEXUSPORT = '8081'
        NEXUS_GRP_REPO = 'vpro-maven-group'
        NEXUS_LOGIN = 'nexuslogin'
        SONARSERVER = 'sonarserver'
        SONARSCANNER = 'sonarscanner'
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install' //we use -s settings.xml in order to use our nexus repo and not maven repo
            }
            post { //when the steps is completed what to do
                success {
                    echo 'Now Archiving.'
                    archiveArtifacts artifacts: '**/*.war'  //archiveArtifacts plugin : archive everyting that endds with .war
                }
            }
        
        }
        stage('Unit Test'){
            steps {
                sh 'mvn -s settings.xml test' //shell command ,unittest ,will generate a report that we use later and upload on sonarqube server
            }
        }


        stage('Checksyle Analysis:Code analysis tool'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle' //code analysis tool , suggests for best practices 
            }
        }

        
        stage('Sonar Analysis') {
            environment {
            scannerHome = tool "${SONARSCANNER}" //globar variable into local variable(scannerHome)
            }
            
            steps {
               withSonarQubeEnv("${SONARSERVER}") {
                   sh '''${scannerHome}/bin/sonar-scanner -Dsonar.projectKey=Flidoxproject \
                   -Dsonar.projectName=Flidoxproject \
                   -Dsonar.projectVersion=1.0 \
                   -Dsonar.sources=src/ \
                   -Dsonar.java.binaries=target/test-classes/com/visualpathit/account/controllerTest/ \
                   -Dsonar.junit.reportsPath=target/surefire-reports/ \
                   -Dsonar.jacoco.reportsPath=target/jacoco.exec \
                   -Dsonar.java.checkstyle.reportPaths=target/checkstyle-result.xml'''
              }
            }

        }
        stage("Quality Gate") {
            steps {
                timeout(time: 1, unit: 'HOURS') { //timeout after 1 hour so it stops after 1 hour
                    // Parameter indicates whether to set pipeline to UNSTABLE if Quality Gate fails
                    // true = set pipeline to UNSTABLE, false = don't
                    waitForQualityGate abortPipeline: true
                }
            }
        }

        stage("UploadArtifact"){
            steps{
                nexusArtifactUploader(                                      //jenkins plug in
                  nexusVersion: 'nexus3',
                  protocol: 'http',                                         
                  nexusUrl: "${NEXUSIP}:${NEXUSPORT}",                      //nexus ip and port from enviroment variable
                  groupId: 'QA',                                            //folder QA fron nexus repository
                  version: "${env.BUILD_ID}-${env.BUILD_TIMESTAMP}",        // build id + build timestamp from jenkins plug in
                  repository: "${RELEASE_REPO}",                            // the name i give to nexus repository
                  credentialsId: "${NEXUS_LOGIN}",                          //nexus login credential i saved in jenkins
                  artifacts: [
                    [artifactId: 'flidoxapp',                   //prefix   subfolder           
                     classifier: '',
                     file: 'target/vprofile-v2.war',
                     type: 'war']
                  ]
                )
            }
        }
    post {                                                         
        always{                                                         //always this will be executed 
            echo 'Slack Notification.'   //print message
            slackSend channel: '#jenkins', //slackSend is the plugin we install  + channel name
                color: COLOR_MAP[currentBuild.currentResult],  // jenkins globar variables
                message: "'*${currentBuild.currentResult}':* Job ${env.JOB_NAME} build ${env.BUILD_NUMBER} \n More info at: ${env.BUILD_URL}" //message name 
            }
        }
   

    }
    
} 