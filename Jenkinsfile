pipeline {
    agent any

    stages {
        stage('Prepare Dev Env') {
            parallel {
                stage('Create Dev Conda Env'){                   
                    steps {                    
                        sh '''#!/bin/bash
			    export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
			    docker ps | grep rabbit
			    docker ps | grep redis
			    docker ps | grep postgres
                           '''
                    }                    
                }
             }
        }

            stage('Smoke Test') {
            steps {
				sh '''#!/bin/bash
				export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
                    source activate nameko-devex
                    echo "Start app service ..."
                    make deploy-docker > app.log &
                    sleep 10
                    echo "Start smoketest ..."
                    make smoke-test
				'''
            }
        }
    }
}
