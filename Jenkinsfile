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
                    file: "target/${ArtifactId}-${Version}.war", 
                    type: 'war']], 
                    credentialsId: '4d072d93-b352-4162-bc6a-3d2c5b9e08e5', 
                    groupId: "${GroupId}", 
                    nexusUrl: '172.20.10.142:8081', 
                    nexusVersion: 'nexus3', 
                    protocol: 'http', 
                    repository: "${NexusRepo}", 
                    version: "${Version}"                  

                }
                
            }
        }
        //stage4 : Deploying
        stage ('Deploy') {
            steps {
                echo 'deploying .........'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible Controler', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy.yaml -i /opt/playbooks/hosts', 
                            execTimeout: 120000,                            
                        )
                    
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }

        }

        //stage5 : Deploying build artifacts to docker
        stage ('Deploy to Docker') {
            steps {
                echo 'deploying .........'
                sshPublisher(publishers: 
                [sshPublisherDesc(
                    configName: 'Ansible Controler', 
                    transfers: [
                        sshTransfer(
                            cleanRemote: false,
                            execCommand: 'ansible-playbook /opt/playbooks/downloadanddeploy_docker.yml -i /opt/playbooks/hosts', 
                            execTimeout: 120000,                            
                        )
                    
                    ], 
                    usePromotionTimestamp: false, 
                    useWorkspaceInPromotion: false, 
                    verbose: false)
                    ])
            }

        } 

        //stage 6 
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