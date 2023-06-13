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

    }
    
}