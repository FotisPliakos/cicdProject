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
    }

    stages {
        stage('Build') {
            steps {
                sh 'mvn -s settings.xml -DskipTests install' //we use -s settings.xml in order to use our nexus repo and not maven repo
            }
            post { //when the steps is completed what to do
                echo 'Now Archiving.'
                archiveArtifacts artifacts: '**/*.war'  //archiveArtifacts plugin : archive everyting that endds with .war
            }
        
        }
        stage('Unit Test'){
            steps {
                sh 'mvn -s settings.xml test' //shell command ,unittest ,will generate a report that will later will upload on sonarqube
            }
        }


        stage('Checksyle Analysis:Code analysis tool'){
            steps {
                sh 'mvn -s settings.xml checkstyle:checkstyle' //code analysis tool , and suggest for best practices 
            }
        }
    
    }
    
}