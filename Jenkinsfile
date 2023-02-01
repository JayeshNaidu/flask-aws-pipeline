env.name = 'xcel2json'
pipeline{
    agent{
        label "node"
    }
    stages{
        stage("pulling code"){
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
        stage("building image"){
            steps{
                //changing dir
                sh "pwd"
                sh "ls"
                dir('flask-aws-pipeline') {
                    sh "pwd"
                    sh "ls -a"
                    //building the docker image
                    sh "sudo docker build -t $name ."
                    
                }
                
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
        stage("Running the container"){
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
