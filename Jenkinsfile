pipeline {
    agent any

    stages {
        stage('Prepare Dev Env') {
            parallel {
                stage('Create Dev Conda Env'){                   
                    steps {                    
                        sh '''#!/bin/bash
			    export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
			    source activate nameko-devex
			    sleep 5
                           '''
                    }                    
                }
		  stage('Start backend services'){                   
                  steps {                    
                     sh '''#!/bin/bash
			  export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
			  source activate nameko-devex
			  ./dev_run_backingsvcs.sh
			  sleep 10
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
                    sleep 50
                    echo "Start smoketest ..."
                    ./test/nex-smoketest.sh local
				'''
            }
        }
	    stage('Perf Test') {
            steps {
		     sh '''#!/bin/bash
		    export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
                    source activate nameko-devex
                    echo "Start Perf test..."
                    make perf-test
				'''
            }
        }
    }
}
