pipeline {
    //Directives
    agent any

    tools {
        maven 'maven'
    }

    stages {
        // Specify various stage with in stage

        //stage1 : Build
        stage ('Build') {
            steps {
                sh 'mvn clean install package'
            }

        }
        //stage2 : Testing
        stage ('Test') {
            steps {
                echo 'testing.......'
            }
        }

        //stage3: Publish the artifacts to nexus
        stage ('Publish to Nexus') {
            steps {
                nexusArtifactUploader artifacts: [[artifactId: 'VinayDevOpsLab', classifier: '', file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', type: 'war']], credentialsId: 'f92b62ad-7e02-4391-85e7-b369aca9fd6e', groupId: 'com.vinaysdevopslab', nexusUrl: '172.20.10.149:8081', nexusVersion: 'nexus3', protocol: 'http', repository: 'VinaysDevOpsLab-SNAPSHOT ', version: '0.0.4-SNAPSHOT'
            }
        }
        //stage3 : Deploying
        stage ('Deploy') {
            steps {
                echo 'deploying .........'
            }

        }
    }
}