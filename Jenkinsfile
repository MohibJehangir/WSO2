pipeline {

    agent any
    environment { 
        PATH = "/home/osboxes/WSO2/apictl:$PATH"
    } 
    options {
        buildDiscarder logRotator( 
                    daysToKeepStr: '16', 
                    numToKeepStr: '10'
            )
    }

    stages {

    /*    stage('Setup Environment for APICTL') {
            steps {
			sh("bash -c \"chmod +x ${env.WORKSPACE}/apictl/*\"")
                sh """#!/bin/bash
				rm -rf /var/lib/jenkins/.wso2apictl
                ENVCOUNT=\$($WORKSPACE/apictl/apictl get envs --format {{.}} | wc -l)
                if [ "\$ENVCOUNT" != "1" ]; then
                    $WORKSPACE/apictl/apictl add env prod --apim https://localhost:9443/carbon/
                fi
                """
            }
        }	*/
		
		stage('Setup Environment for APICTL') {
            steps {
			sh("bash -c \"chmod +x ${env.WORKSPACE}/apictl/*\"")
                sh """#!/bin/bash
				rm -rf /var/lib/jenkins/.wso2apictl
				cd $WORKSPACE/apictl/
				./apictl version
                ./apictl add env dev --apim https://localhost:9443 
                """
            }
        }	

        stage('Deploy APIs To "Production" Environment') {
            steps {
                sh("bash -c \"chmod +x ${env.WORKSPACE}/apictl/*\"")
				sh """
				cd $WORKSPACE/apictl/
                ./apictl login dev -u admin -p admin
				./apictl init -f openapi --oas $WORKSPACE/apictl/openapi.yaml
				./apictl import api -f $WORKSPACE/openapi -e dev
				./apictl change-status api --action 'Publish' -n SwaggerPetstore -v 1.0.5 -e dev
                """
            }
        }
    }   
}
