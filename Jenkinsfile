pipeline {
    agent any
    parameters {
        string(defaultValue: 'default', name: 'suppressions', trim: false)
        string(defaultValue: 'default', name: 'checks', trim: false)
        string(defaultValue: 'false', name: 'show_diff', trim: true)
        string(defaultValue: 'true', name: 'continue_on_error', trim: true)
        string(defaultValue: 'all', name: 'check_type', trim: false)
        string(defaultValue: '', name: 'helm_chart', trim: false)
        string(defaultValue: 'v3', name: 'helm_version', trim: true)
        string(defaultValue: '', name: 'chart_parameters', trim: false)
    }
    environment {
    CHKK_ACCESS_TOKEN = credentials("CHKK_ACCESS_TOKEN")
    }
    stages {
        stage("Chkk") {
            steps {
               sh '''#!/bin/bash
               if [[ ${helm_version} = "v3" ]]; then
                   echo "Using helm v3";
                   curl -fsSL -o get_helm.sh https://raw.githubusercontent.com/helm/helm/master/scripts/get-helm-3;
                   chmod 700 get_helm.sh;
                   ./get_helm.sh;
               elif [[ {helm_version} = "v2" ]]; then
                   echo "Using helm v2";
                   curl -sSL https://get.helm.sh/helm-v2.16.6-linux-amd64.tar.gz | tar zx;
                   sudo cp linux-amd64/helm /usr/local/bin/helm;
               fi;
               VERSION=$(curl -sS https://get.chkk.dev/cli/latest-beta.txt)
               curl -Lo chkk https://get.chkk.dev/${VERSION}/chkk-darwin-amd64;
               export CHKK_ACCESS_TOKEN=$CHKK_ACCESS_TOKEN;
               chmod +x chkk;
               helm template ${helm_chart} ${chart_parameters} | ./chkk -f - -c ${checks} -s ${suppressions} --show-diff=${show_diff} --continue-on-error=${continue_on_error} --check-type=${check_type}
               '''
            }
        }
        
    }   
}
