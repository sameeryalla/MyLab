pipeline {
    //Directives
    agent any

    tools {
        maven 'maven'
    }

    environment {
        ArtifactId = readMavenPom().getArtifactId()
        Version = readMavenPom().getVersion()
        Name = readMavenPom().getName()
        GroupId = readMavenPom().getGroupId()
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
                script{
                    def NexusRepo = version.endsWith("SNAPSHOT") ? "VinaysDevOpsLab-SNAPSHOT" : "VinaysDevOpsLab-RELEASE"

                    nexusArtifactUploader artifacts: 
                    [[artifactId: "${ArtifactId}", 
                    classifier: '', 
                    file: 'target/VinayDevOpsLab-0.0.4-SNAPSHOT.war', 
                    type: 'war']], 
                    credentialsId: 'f92b62ad-7e02-4391-85e7-b369aca9fd6e', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.149:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"                  

                }
                
            }
        }
        //stage3 : Deploying
        stage ('Deploy') {
            steps {
                echo 'deploying .........'
            }

        }

        //stage 4 
        stage ('Print Environment variables') {
            steps {
                echo "Atrtifact ID is '${ArtifactId}'"
                echo "Version is '${Version}'"
                echo "GroupID is '${GroupId}'"
                echo "Name is '${Name}'"
            }
        }
    }
}