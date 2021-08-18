pipeline {
    agent any
    parameters {
        string(defaultValue: 'default', name: 'suppressions', trim: false)
        string(defaultValue: 'default', name: 'checks', trim: false)
        string(defaultValue: 'false', name: 'show_diff', trim: true)
        string(defaultValue: 'true', name: 'continue_on_error', trim: true)
        string(defaultValue: '', name: 'kubernetes_manifest', trim: false)
        string(defaultValue: 'all', name: 'check_type', trim: false)
    }
    environment {
    CHKK_ACCESS_TOKEN = credentials("CHKK_ACCESS_TOKEN")
    }
    stages {
        stage("Chkk") {
            steps {
               sh '''#!/bin/bash
               VERSION=$(curl -sS https://get.chkk.dev/cli/latest.txt)
               curl -Lo chkk https://get.chkk.dev/${VERSION}/chkk-darwin-amd64;
               export CHKK_ACCESS_TOKEN=$CHKK_ACCESS_TOKEN;
               chmod +x chkk;
               ./chkk -f ${kubernetes_manifest}  -c ${checks} -s ${suppressions} --show-diff=${show_diff} --continue-on-error=${continue_on_error} --check-type=${check_type}
               '''
               
            }
        }
        
    }   
}
