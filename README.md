# TASK : To automate the deployment of _ceros-ski_ Node.js application to an AWS EC2 instance
### TECHNOLOGIES USED: Docker, Docker-compose, Terraform, EC2 (free tier; Ubuntu 18.04 LTS), Nginx

**Maintainer**: app-dev@ceros.com
    
This is the documentation for deploying the ceros-ski app. This set up fulfils all the general requirements for the deployment as stated in the challenge document. It also fulfils items 1 and 2 in the bonus section of the document.

The App traffic flow is as below:

<img width="723" alt="Ceros-ski App Traffic Diagram" src="https://user-images.githubusercontent.com/37908685/56900121-0054c800-6a8d-11e9-9e5b-33cb8fb25a3b.png">


**The deployment for the app takes place in two parts:**
1. Infrastructure build using Terraform
2. App deployment on infrastructure using a bash script.


**1. Infrastructure build using Terraform** 
  
The infrastructure for the app is built using the concept of Infrastructure as Code on AWS using Terraform. The files used for this are:
  - **main.tf:** This contains the terraform config that builds the AWS infrastructure such as VPC, SG, IGW, subnet and the EC2 instance
  - **variables.tf:** This contains a listing of variables their default values
  - **output.tf:** This contains some important output values, such as the public IP of the instance through which the app would  be accessed through the internet.


**2. App deployment on infrastructure using a bash script**
  
After terraforms builds the EC2 Instance, it calls the bash script, **bootstrap.sh**, to deploy the app.The bootstrap script does the following:
  
  - Installs Docker community edition and other utilities
  - Starts and enables Docker (if not already started)
  - Installs Docker-compose used for deploying multiple containers which are  linked together
  - Builds the nodejs app into a Docker image
  - Creates the docker-compose.yaml file
  - Creates the nginx loadbalancer config file
  - Deploys multiple app containers frontended and loadbalanced by the nginx container


**To deploy the app, do the following:**
  - Have or create an AWS account
  - Have the app folder copied onto a deploy EC2 instance
  - Have a valid Access key ID and Secret access key set up with the appropriate permissions on AWS IAM
  - Have the Access key ID and Secret access key securely set to the appropriate variable in the variable.tf file, e.g. using environment variables (credentials should not be saved in (publicly accessible) files)
  - Have Terraform installed and initialised
  - In your console, type terraform apply' then the press enter key

After terraform concludes the deployment, it outputs the public IP of the EC2 instance through which the application can be accessed on the internet.

NB: One way to do the last bonus challenge, i.e. update app without downtime, would be to spin up new (green) deployment and                                                have DNS (say Route 53) switch traffic from the old (blue) deployment to the new deployment. There should be no downtime for   this method.
