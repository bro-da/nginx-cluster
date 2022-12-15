pipeline {
    agent any 
     environment {
        DATE = new Date().format('yy.M')
        TAG = "${DATE}.${BUILD_NUMBER}"
        // DOCKERHUB_CREDENTIALS=credentials('dockerhub')
    }
    stages {
        stage('helm pull') { 
            steps {
                sh 'helm repo add nginx-stable https://helm.nginx.com/stable'
                sh 'helm pull nginx-stable/nginx-ingress' 
            }
        }
        stage('extract'){
            steps{
                sh 'tar -zxvf nginx-ingress-*.tgz'
                sh 'rm -rf nginx-ingress-*.tgz'
            }
        }
        stage("change values in chart.yaml"){
            steps{
                //  sh 'sed -i "s/tag: ""/tag: "$USER"/g" mavenhelm/values.yaml'
                sh 'echo "changed values"'
            }
        }
        stage("compress file for exporting"){
            steps{
                sh 'tar cvzf nginx-cluster.${BUILD_NUMBER}.tgz nginx-ingress '
                // sh 'docker tag nginx-ingress:latest 976846671615.dkr.ecr.us-east-1.amazonaws.com/nginx-ingress:latest'
            }
        }
        stage("aws login "){
            steps{
                //  sh 'aws ecr-public get-login-password --region us-east-1 | docker login --username AWS --password-stdin public.ecr.aws/l2m3f3d0'
                sh 'aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 976846671615.dkr.ecr.us-east-1.amazonaws.com'
                sh 'helm push nginx-cluster.${BUILD_NUMBER}.tgz oci://976846671615.dkr.ecr.us-east-1.amazonaws.com'
            }
        }
    }
}

