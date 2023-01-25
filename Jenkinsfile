pipeline {
    agent any

    stages {
        stage('Prepare Dev Env') {
            parallel {
                stage('Create Dev Conda Env'){                   
                    steps {                    
                        sh '''#!/bin/bash
			    export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
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
                    ./dev_run.sh gateway.service orders.service products.service > app.log &
                    sleep 5
                    echo "Start smoketest ..."
                    ./test/nex-smoketest.sh local
				'''
            }
        }
    }
}
