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
			    docker ps | grep rabbit
			    docker ps | grep redis
			    docker ps | grep postgres
                           '''
                    }                    
                }
		  stage('Start backend services'){                   
                  steps {                    
                     sh '''#!/bin/bash
			  export "PATH=$PATH:/var/lib/jenkins/miniconda3/bin"
			  source activate nameko-devex
			  make deploy-docker > app.log &
			  sleep 5
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
		    sleep 5
				'''
            }
        }
    }
}
