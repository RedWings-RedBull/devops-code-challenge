# Overview #

 This repository contains the source code for a React frontend, an Express backend, and an AWS CloudFormation template that deploys a containerized version of the 2 Apps on Amazon Elastic Container Service. 

Further down in this document you will be provided steps to replicate this containerized deployment within your own AWS environment.

## NOTE ##
The included AWS containerized deployment uses ECS Fargate, a serverless, container management option for AWS Elastic Container Service. One limitation of ECS Fargate is it only allows containers of the networking type "awsvpc". Due to this networking limitation, when the frontend container sends the GET request to the backend container, the backend container sees the request coming from the public IP address of the container's interface, instead of localhost. 

The resulting problem is if you connect to the frontend with default settings in Google Chrome, you will see a failure to connect error. The GET request will fail with the following error: "cross-origin resource sharing Error: InsecurePrivateNetwork". Google Chrome is blocking the request. This can be unblocked using the following workaround.

### Google Chrome Workaround ###
Navigate to chrome://flags/#block-insecure-private-network-requests and change the "Block insecure private network requests" setting to "Disabled". You will now see that the front end now has a successful response.


## Replicating the containerized environment in AWS ##

### Requirements ###
1) Have an AWS account created and ready for resource provisioning. 

2) Have AWS CLI installed and configured for use on your AWS account.

3) Have the AWS subnet ID of an AWS subnet with your account.


### Steps ###
1) Clone the repo to your system with the command below.

	git clone challenge https://github.com/RedWings-RedBull/devops-code-challenge.git

2) Open your terminal and navigate so that the "deployment template" folder is your current working directory.

3) Enter the command below, except replaced SUBNETIDGOESHERE with the subnet ID you wish to deploy within.

	aws cloudformation create-stack --stack-name interview-challenge-deployment --template-body file://./CloudFormationTemplate.yaml --capabilities CAPABILITY_NAMED_IAM --parameters 'ParameterKey=SubnetID,ParameterValue=SUBNETIDGOESHERE'

	Example:
	aws cloudformation create-stack --stack-name interview-challenge-deployment --template-body file://./CloudFormationTemplate3.yaml --capabilities CAPABILITY_NAMED_IAM --parameters 'ParameterKey=SubnetID,ParameterValue=subnet-01cd23680cc9f6962'

	Successful output looks as follows:
	{"StackId": "arn:aws:cloudformation:us-east-2:86423434432:stack/interview-challenge-deployment/7759970-e052-11ec-be18-026d64e7ea2"
}

4) Go to the AWS CloudFormation console and view the Stacks. Monitor the status of the interview-challenge-deployment stack until it says "CREATE_COMPLETE". 

5) Once the stack is complete, navigate to the AWS Elastic Container Service console and click on the interview-challenge-ecs-cluster cluster. Click on the Tasks tab, then click on the only available task, and note the Public IP on the next page.

6) Navigate to http://PUBLICIP:3000, and as long as you have the aforementioned Google Chrome workaround in place, you will see the results of the backend request.

