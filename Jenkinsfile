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
                        // AWS 명령어로 ECR 로그인 토큰을 가져오고 env.token에 저장
                        def ecr_token = sh(script: 'aws ecr get-login-password --region $AWS_REGION', returnStdout: true)
                        env.token = ecr_token
                        
                        // 디버깅: ECR 토큰 출력
                        echo "ECR Token: ${env.token}"
                    }
                }
            }
        }
        stage("helm package & push to ecr") {
            steps {
                container("helm"){
                    script {
                        sh '''#!/bin/bash
                            helm registry login --username AWS --password-stdin 866477832211.dkr.ecr.${AWS_REGION}.amazonaws.com <<< "${token}"
                        '''
                    }
                }
            }
        }
    }
}
