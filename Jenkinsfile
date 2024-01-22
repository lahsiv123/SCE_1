pipeline {
    agent any 
    
    stages{
        stage("Clone Code"){
            steps {
                echo "Cloning the code"
                git url:"https://github.com/lahsiv123/SCE_1.git"
            }
        }
        stage("Build"){
            steps {
                echo "Building the image"
            }
        }
        stage("Push to AWS ECR"){
            steps {
                withAWS(roleAccount:'534418046322', role:'ecr_registry_ec2_fullaccess') {

                echo "Pushing the image to AWS ECR"
                sh "aws ecr get-login-password --region us-east-1 | docker login --username AWS --password-stdin 534418046322.dkr.ecr.us-east-1.amazonaws.com"
                sh "docker build -t django_rep ."
                sh "docker tag django_rep:latest 534418046322.dkr.ecr.us-east-1.amazonaws.com/django_rep:latest"
                sh "docker push 534418046322.dkr.ecr.us-east-1.amazonaws.com/django_rep:latest"
            }

                }
            }
        }
        stage("Deploy"){
            steps {
                echo "Deploying the container"
                sh "docker-compose down && docker-compose up -d"
                
            }
        }
    }
}
