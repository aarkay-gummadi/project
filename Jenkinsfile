pipeline{

    agent any
    environment {
       DOCKERHUB_CREDENTIALS = credentials('Docker_cred')
    }

    stages {
        stage('SCM Checkout'){
            steps {
                // remove folder if already exist
                sh 'rm -rf project'

                // clone the repository from github
                sh 'git clone https://github.com/aarkay-gummadi/project.git'
            }
        }
        

        stage ('Building New Docker Image') {
            steps {
                script {
                    dir('./docker') {
                        //  Building new image
                        sh 'docker image build -t rajkumar207/flask:latest .'
                        echo "Image successfully built"
                    }
                }
            }
        }

        stage('Updating Image On Dockerhub'){
            steps {
                script {
                    //  Pushing Image to Repository
                    sh 'echo $DOCKERHUB_CREDENTIALS_PSW | docker login -u $DOCKERHUB_CREDENTIALS_USR --password-stdin'
                    
                    sh 'docker push rajkumar207/flask:latest'
                
                    echo "Image has been updated on dockerhub"
                }
            }
        }

        stage('Starting Kubernetes') {
            steps {
                script{
                    
                    sh 'minikube start'
                    echo 'kubernetes started successfully'
                    
                }
            }
        }
        
        stage('Terraform init') {
            steps {
                script{
                   sh 'terraform init'
                }
            }
        }

        stage('Terraform Apply') {
            steps {
                script{
                     sh 'terraform apply -auto-approve'
                }
            }
        }

        stage('Pods Info') {
            steps {
                script{
                    
                    sh 'kubectl get po -A'
                    
                }
            }
        }

        stage('Deployment Info') {
            steps {
                script{
                    
                    sh 'kubectl get deployment'
                    
                }
            }
        }

        stage('Cluster Info') {
            steps {
                script{
                    
                    sh 'kubectl get all'
                    
                }
            }
        }
        


    }
}
