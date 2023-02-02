env.name = 'xcel2json'
env.AWS_ACCOUNT_ID = '342488151234'
env.AWS_DEFAULT_REGION= 'us-east-1'
env.IMAGE_TAG= 'latest'
env.IMAGE_REPO_NAME= 'flask-aws-build'
env.FINAL_REPO_URI= ''
pipeline{
    agent{
        label "node"
    }
    stages{
        stage("Cloning Git Repo"){
            steps{
                //we are pulling the code from github
               git url: 'https://github.com/JayeshNaidu/flask-aws-pipeline.git' , branch: 'main'
            }
            post{
                always{
                    echo "complete stage 1"
                }
                success{
                    echo "code pulled"
                }
                failure{
                    echo "failed fetch"
                }
            }
        }
        stage("Building Image"){
            steps{
                //changing dir
                sh "pwd"
                sh "ls"
                sh "sudo docker build -t $name ."
                
            }
            post{
                always{
                    echo "complete stage 2"
                }
                success{
                    echo "built image"
                }
                failure{
                    echo "failed building image"
                }
            }
        }
        stage("Running Container"){
            steps{
                //running container
                sh "sudo docker run -p 3000:5000 --name $name -d $name"
                
            }
            post{
                always{
                    echo "complete stage 3"
                }
                success{
                    echo "container running"
                }
                failure{
                    echo "run failed"
                }
            }
        }

        stage("Login to AWS ECR"){
            steps{
                //login to ecr
                sh "aws ecr get-login-password --region $AWS_DEFAULT_REGION | docker login --username AWS --password-stdin $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com"
                
            }
            post{
                always{
                    echo "stage 4 completed"
                }
                success{
                    echo "Authenticated to ECR repo"
                }
                failure{
                    echo "Authentication failed"
                }
            }
        }

        stage("Pushing the image to AWS ECR"){
            steps{
                //Tagging image
                sh "docker tag $IMAGE_REPO_NAME:$IMAGE_TAG $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG"
                
                //Pushing to ECR repo
                sh "docker push $AWS_ACCOUNT_ID.dkr.ecr.$AWS_DEFAULT_REGION.amazonaws.com/$IMAGE_REPO_NAME:$IMAGE_TAG"
            }
            post{
                always{
                    echo "stage 5 completed"
                }
                success{
                    echo "Successfully pushed image to ECR repo"
                }
                failure{
                    echo "push failed"
                }
            }
        }
    }
    post{
        always{
            echo "all stages completed"
        }
        success{
            echo "all stages completed successfully"
        }
        failure{
            echo "some or all stages failed "
        }
    }
} 
