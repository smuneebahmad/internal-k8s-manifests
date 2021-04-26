pipeline {
    agent any
    parameters {
        string(defaultValue: 'default', name: 'skip_checks', trim: false)
        string(defaultValue: 'default', name: 'enable_checks', trim: false)
        string(defaultValue: 'false', name: 'hide_diff', trim: true)
        string(defaultValue: 'true', name: 'continue_on_error', trim: true)
        string(defaultValue: 'kubernetes-sample-manifest.yaml', name: 'kubernetes_manifest', trim: false)
    }
    environment {
    CHKK_ACCESS_TOKEN = credentials("CHKK_ACCESS_TOKEN")
    }
    stages {
        stage("Chkk") {
            steps {
               sh '''#!/bin/bash
               curl -Lo chkk https://chkk-artifacts-downloads.s3.amazonaws.com/dl/v0.0.1/chkk-darwin-amd64;
               export CHKK_ACCESS_TOKEN=$CHKK_ACCESS_TOKEN;
               chmod +x chkk;
               ./chkk -f ${kubernetes_manifest}  -r ${enable_checks} -s ${skip_checks} --hide-diff=${hide_diff} --continue-on-error=${continue_on_error}
               '''
               
            }
        }
        
    }   
}
