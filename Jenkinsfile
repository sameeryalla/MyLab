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
        //stage3 : Deploying
        stage ('Deploy') {
            steps {
                echo 'deploying .........'
            }

        }
    }
}