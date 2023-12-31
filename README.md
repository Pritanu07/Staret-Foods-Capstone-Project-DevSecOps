DevSecOps-CI/CD Pipeline-Capstone project

Company Profile:

Staret Foods is specialized for the elderly based on their dietary needs. Staret Foods mainly serves Singapore. Their main target group is people who are over 60 and have special dietary needs. Even though there are many food delivery services in the market, Staret Foods is the monopoly in the elder’s food business. Since its focus group is the elderly, Staret Foods wants to maintain their website as easily accessible and more secure from vulnerabilities. I have been approached with the above requirement from Staret Foods, and my suggestion is to use ECS with Terraform infrastructure using the Far Gate (serverless) platform. As a cloud infrastructure engineer, I extended my support to make their application more secure and reliable based on the cloud DevSecOps approach in an automated CI/CD pipeline. Technical recommendations are as follows in detail:

Cloud infrastructure dependencies:
AWS ECS with Far gate is a serverless computing platform that makes running containerized services on AWS easier than ever before. With Far gate, a user simply defines the compute resources, such as CPU and memory, that a service will need to run, and Far gate will manage where to run the container behind the scenes. There is no point where setting up an EC2 instance is required. Terraform is an infrastructure-as-code tool created by Hashicorp to make handling infrastructure more straightforward and manageable.

ECR-ECS- CI/CD pipeline workflow diagram


![image](https://github.com/Pritanu07/staret-food-capstone-project/assets/127757033/4770df4e-0782-41a2-8242-f0ba49e5370c)




                                                                  
 
Creating ECR, Building docker image and push the image to ECS in Terraform Infrastructure

This guide provides step-by-step instructions on building a Docker application, pushing it to Amazon Elastic Container Registry (ECR), deploying it to Amazon Elastic Container Service (ECS) using Terraform, and handling common errors.

# Prerequisites
1.	Prerequisites
2.	Docker Image Creation and Push
3.	ECS Deployment Using Terraform
4.	Resource Destruction
5	Handling ECR Image Deletion Error
6.	Conclusion

Before you begin, ensure you have:
	An AWS account with the necessary permissions.
	Docker installed on your local machine. You can download Docker from Docker's official website.
	Terraform installed on your local machine. You can download Terraform from Terraform's official website.
Docker Image Creation and Push
# Set Up the Docker Image:
	Create a directory for your Docker project and navigate to it in your terminal:
	mkdir staret-foods-capstone-project
           cd staret-foods-capstone-project
	Inside this directory, create a Dockerfile with the content in the example Dockerfile.
	Build the Docker image locally:
           docker build -t staret-foods-capstone-project .
# Push Docker Image to Amazon ECR:
	Log in to Amazon ECR using the AWS CLI:
           aws ecr get-login-password --region ap-southeast-1 | docker login --username AWS --password-stdin 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com
	Tag the Docker image with your ECR repository URI:
           docker tag staret-foods-capstone-project:latest 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/priya-repo:latest
# push the tagged Docker image to Amazon ECR:

           docker push 255945442255.dkr.ecr.ap-southeast-1.amazonaws.com/priya-repo:latest
           
# ECS Deployment Using Terraform:

Set Up the Terraform Configuration:
	Create a terraform.tf file in your project directory and add the Terraform configuration from the example Terraform configuration.
Deploy ECS Service:
	Run the following Terraform commands to deploy the ECS service:
	terraform init
           terraform apply
           
1.https://ap-southeast-1.console.aws.amazon.com/ecr/repositories/private/255945442255/priya-devsecops-application?region=ap-southeast-1

2.https://ap-southeast-1.console.aws.amazon.com/ecs/v2/clusters/priya-devsecops8-application-cluster/services/priya-devsecops-application-service/health?region=ap-southeast-1

3.https://ap-southeast-1.console.aws.amazon.com/ecs/v2/clusters/priya-devsecops8-application-cluster/tasks/8db87130d4e446c4923b82dc156d0952/configuration?region=ap-southeast-1&selectedContainer=priya-devsecops-application

# Test for vulnerability scan, fix bug and scan dependencies :

Unit Test: Run unit test in CI before docker image deployment npm install -jest -save-dev npm test
Adding Security in CI/CD pipeline :
1.	Vulnerability scan --> npm install npm audit
2.	Secret key access with IAM permsission --> parameter store ACCESS_KEY: ${ssm:ACCESS_KEY} access_key: process.env.ACCESS_KEY
3.	snyk-Open-source dependencies,container images & Iac configuration --> npm install -g snyk, snyk auth, snyk monitor
4.	
# Resource Destruction
Destroy Resources:
	If you no longer need the resources, navigate to the project directory and run:
           terraform destroy
           
# Handling ECR Image Deletion Error

If You Encounter Deletion Error:
If you encounter an error when trying to delete an ECR image with a specific tag, such as latest, you can use the following AWS CLI command to forcefully delete it:                                                                                                                                                                                               aws ecr batch-delete-image --repository-name priya-repo --image-ids imageTag=latest

Conclusion

By following this guide, you've learned how to build a Docker image, push it to Amazon ECR, deploy an ECS service using Terraform, and manage resources effectively. This approach provides a streamlined way to manage your containerized applications on AWS.
Remember to replace placeholder values with your actual configuration details and paths. For more advanced scenarios, you can extend this configuration further.

