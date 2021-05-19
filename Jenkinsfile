pipeline {
    agent any
    parameters {
        string(defaultValue: 'default', name: 'skip_checks', trim: false)
        string(defaultValue: 'default', name: 'enable_checks', trim: false)
        string(defaultValue: 'false', name: 'show_diff', trim: true)
        string(defaultValue: 'true', name: 'continue_on_error', trim: true)
        string(defaultValue: '', name: 'kubernetes_manifest', trim: false)
        string(defaultValue: 'reliability', name: 'check_type', trim: false)
    }
    environment {
    CHKK_ACCESS_TOKEN = credentials("CHKK_ACCESS_TOKEN")
    }
    stages {
        stage("Chkk") {
            steps {
               sh '''#!/bin/bash
               curl -Lo chkk https://downloads.chkk.dev/v0.0.1/chkk-linux-amd64;
               export CHKK_ACCESS_TOKEN=$CHKK_ACCESS_TOKEN;
               chmod +x chkk;
               ./chkk -f ${kubernetes_manifest}  -r ${enable_checks} -s ${skip_checks} --show-diff=${show_diff} --continue-on-error=${continue_on_error} --check-type=${check_type}
               '''
               
            }
        }
        
    }   
}
