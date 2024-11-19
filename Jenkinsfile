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
        
        stage("login") {
            steps {
                container("aws"){
                    script {
                        sh """
                            aws sts get-caller-identity
                            aws ecr get-login-password --region ${AWS_REGION}
                            token=$(aws ecr get-login-password --region "$AWS_REGION")
                            echo $token
                        """
                    }
                }
            }
        }
    }
}
