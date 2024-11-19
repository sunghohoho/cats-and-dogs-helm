pipeline {
    agent {
        label "helm"
    }
    
    environment {
        AWS_REGION = 'ap-northeast-2'  
        AWS_HELM_REPO = '866477832211.dkr.ecr.ap-northeast-2.amazonaws.com/helm-charts/'
    }
    
    stages {
        stage("Checkout") {
            steps {
                checkout scm
            }
        }
        
        stage("ecr get token") {
            steps {
                container("aws"){
                    script {
                        sh '''#!/bin/bash
                            aws sts get-caller-identity
                            env.ecr_token=$(aws ecr get-login-password --region $AWS_REGION)
                            echo "Token: $ecr_token"
                        '''
                    }
                }
            }
        }
        stage("helm package & push to ecr") {
            steps {
                container("helm"){
                    script {
                        sh '''#!/bin/bash
                            helm registry login --username AWS --password-stdin 866477832211.dkr.ecr.${AWS_REGION}.amazonaws.com <<< "$ecr_token"
                        '''
                    }
                }
            }
        }
    }
}
