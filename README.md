# Terraform Essentials Lab Cheat Sheet
### Terraform Labs Pre-requisites
1. Basic understanding of Linux Commands.
2. Basic knowledge of a Cloud platform such as AWS.
3. Good to have AWS-Free Tier Account for Practice.
## Table Of Contents
* [Lab-1: Creating an EC2 Instance in AWS and Installing Terraform](https://github.com/janjiralakirankumar/TerraformEssentials#lab-1-creating-an-ec2-instance-in-aws-and-installing-terraform)
* [Lab-2: AWS EC2 instance creation using Terraform Variables](https://github.com/janjiralakirankumar/TerraformEssentials#lab-2-aws-ec2-instance-creation-using-terraform-variables)
* [Lab-3 : Using Output Feature](https://github.com/janjiralakirankumar/TerraformEssentials#lab-3--using-output-feature)
* [Lab-4 : Remote State using Amazon Simple Storage Service](https://github.com/janjiralakirankumar/TerraformEssentials#lab-4--remote-state-using-amazon-simple-storage-service)
* [Lab-5: Launching VPC and EC2 Instance](https://github.com/janjiralakirankumar/TerraformEssentials#lab-5-launching-vpc-and-ec2-instance)
* [Lab-6: Launching Auto-Scaling services](https://github.com/janjiralakirankumar/TerraformEssentials#lab-6-launching-auto-scaling-services)
* [Lab-7: Creating a MySQL Database with RDS](https://github.com/janjiralakirankumar/TerraformEssentials#lab-7-creating-a-mysql-database-with-rds)
* [Lab-8: Creating IAM Users, Groups using Terraform.](https://github.com/janjiralakirankumar/TerraformEssentials#lab-8-creating-iam-users-groups-using-terraform)
* [Lab-9: Creating AWS resources using terraform modules](https://github.com/janjiralakirankumar/TerraformEssentials#lab-9-creating-aws-resources-using-terraform-modules)
* [Frequently used Terraform Commands with Explanation](https://github.com/janjiralakirankumar/TerraformEssentials#frequently-used-terraform-commands-with-explanation)
* [Reference Links](https://github.com/janjiralakirankumar/TerraformEssentials#reference-links)

## Lab-1: Creating an EC2 Instance in AWS and Installing Terraform

To begin with Lab-1, Login to AWS Console.

#### Task-1: Installing Terraform on Ubuntu 20.04 operating system

* Launch a **t2.micro** instance with OS version as **Ubuntu 20.04** in North Virginia (us-east-1) Region.
* Use tag "**Name:Terraform-Server**"
* In security groups, include ports **22** and **80**. 
* login with username as "ubuntu".

Once the EC2 is ready, follow the below Commands:
```
sudo hostnamectl set-hostname terraform
bash
```
```
sudo apt update
```
```
sudo apt install wget unzip -y
```
```
wget https://releases.hashicorp.com/terraform/1.5.7/terraform_1.5.7_linux_amd64.zip
```
```
unzip terraform_1.5.7_linux_amd64.zip
```
```
ls
sudo mv terraform /usr/local/bin
```
```
ls
terraform
```
```
terraform -v
```

#### Task-2: Install Required Packages and login to Ubuntu server using Credentials. 
```
sudo apt-get install python3-pip -y
```
```
sudo pip3 install awscli
```
```
aws configure
```
* When it prompts for Credentials, Enter the Keys as example shown below
  
| **Access Key ID.** | **Secret Access Key ID.** |
| ------------------ | ------------------------- |
| AKIAIOSFODNN7EXAMPLE | wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY |

##### Note: If not available generate Credentials from AWS IAM Service
Once LoggedIn check the account access
```
aws s3 ls
```
Or Use below command to check whether it is authenticated.
```
aws iam list-users
```
#### Task-3: Now we are ready to perform the labs
Create directory as Lab1 & create local file
```
mkdir Lab1 && cd Lab1
```
```
vi local.tf
```
```
resource "local_file" "myfile" {

  filename = "/home/ubuntu/test.txt"
  content  = "wel come to terraform"
}
```
```
terraform init
```
```
terraform plan
```
```
terraform apply
```
```
terraform destroy
```
#### Task-4: Launching your first AWS EC2 instance using Terraform 
```
cd ~
mkdir terraform-labs && cd terraform-labs/
```
```
vi example.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  profile = "default" # This line is not mandatory.
  region  = "us-east-2"
}

resource "aws_instance" "example" {
  ami           = "ami-07efac79022b86107"
  instance_type = "t2.micro"
  tags = {
    Name = "Yourname-TF-1"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply
```
```
ls
cat terraform.tfstate
```
Now, Replace the AMI id to **Amazon Linux AMI 2023** by Copy pasting from below.
```
ami-02a89066c48741345
```
```
vi example.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws" {
  profile = "default"
  region  = "us-east-2"
}

resource "aws_instance" "example" {
  ami           = "ami-02a89066c48741345"
  instance_type = "t2.micro"
  tags = {
    Name = "Yourname-TF-1"
  }
}
```

Save the file using "ESCAPE + :wq!"
```
terraform plan
```
```
terraform apply
```
```
cat terraform.tfstate
```
Use the "terraform destroy" command for cleaning the infrastructure used in this lab
```
terraform destroy
```

## Lab 2: AWS EC2 instance creation using Terraform Variables


#### Task-1: Create EC2 instance using variables 
```
cd /home/ubuntu/terraform-labs/
```
```
mkdir variables-lab && cd variables-lab/
```
Now Create four Files ie. **provider.tf,** **vars.tf,** **terraform.tfvars,** **instance.tf.**
```
vi provider.tf
```
Add the given lines, by pressing "INSERT" 
```
provider "aws"{
  access_key=var.AWS_ACCESS_KEY
  secret_key=var.AWS_SECRET_KEY
  region=var.AWS_REGION
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
Add the given lines, by pressing "INSERT" 
```
variable "AWS_ACCESS_KEY"{}
variable "AWS_SECRET_KEY"{}
variable "AWS_REGION"{
  default = "us-east-2"
}
```
Save the file using "ESCAPE + :wq!"
```
vi terraform.tfvars
```
Add the given lines, by pressing "INSERT" 
```
AWS_ACCESS_KEY="< Insert your AWS Access Key >"
AWS_SECRET_KEY="< Insert your AWS Secret Key >"
```
Save the file using "ESCAPE + :wq!"
```
vi instance.tf
```
Add the given lines, by pressing "INSERT" 
```
resource "aws_instance" "terraform_example"{
  ami = "ami-07efac79022b86107"
  instance_type="t2.micro"
  tags = {
    Name = "Lab2-yourname"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform init
```
```
terraform plan
```
```
terraform apply
```

Use the "terraform destroy" command for cleaning the infrastructure used in this lab
```
terraform destroy -auto-approve
```
#### Additional Note (Optional)
* If the name of the tfvars files is anything other than terraform.tfvars then you can use the below command.
```
terraform apply -var-file=<var file name>
```

#### Task-2: Implementing map variables that dynamically fetch AMI based on the Linux distro selected
```
vi instance.tf
```
Delete the existing lines and Add the given lines, by pressing "INSERT" 
```
resource "aws_instance" "terraform_example"{
  ami = var.AMIS[var.Linux_distro]
  instance_type="t2.micro"
  tags = {
    Name = "yourname-lab8B-task2"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
Delete the existing lines and Add the given lines, by pressing "INSERT" 
```
variable "AWS_ACCESS_KEY"{}
variable "AWS_SECRET_KEY"{}
variable "AWS_REGION"{
  default = "us-east-2"
  }

variable "Linux_distro"{
  #description="Please Enter the Linux distro (redhat , ubuntu, amazon )"
  default = "amazon"
  }

variable "AMIS"{
  type=map(string)
  default={
   redhat="ami-0ba62214afa52bec7"
   ubuntu="ami-0fb653ca2d3203ac1"
   amazon="ami-0231217be14a6f3ba"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform fmt
```
```
terraform validate
```
```
terraform plan -var 'Linux_distro=redhat' -out myplan
```
```
terraform apply myplan
```
Use the "terraform destroy" command for cleaning the infrastructure used in this lab
```
terraform destroy
```
## Lab-3 : Using Output Feature 

#### Task-1: Using output feature of Terraform to get the IP Address of EC2 Instance
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/output-variable-lab-v0.13.5.tar.gz
```
```
tar -xvf output-variable-lab-v0.13.5.tar.gz
```
```
cd output-variable-lab/
ls
```
```
cat instance.tf
```
```
cat output.tf
```
```
cat vars.tf
```
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply
```
```
ls 
cat private_ips.txt 
```
```
terraform output Public_ip
```
```
terraform output Private_ip
```
Use the "terraform destroy" command for cleaning the infrastructure used in this lab, remove the directory using rm -rf
```
terraform destroy
```
```
cd ..
```
```
rm -rf output-variable-lab
```
## Lab-4 : Remote State using Amazon Simple Storage Service 

#### Task-1: Create a S3 Bucket using AWS Console 

* Create a new S3 bucket in "**us-east-2**" Region by name: "**yourname-terraform**".
* Select "**ACLs enabled**".
* Uncheck "**block public access**" and select "**I acknowledge that the current settings might result in this bucket and the objects within becoming public**".
* "**Enable versioning**".

To cross Check whether the Bucket is created or not, run the below command in CLI.
```
aws s3 ls 
```
#### Task-2: Configure Remote State
```
cd /home/ubuntu/terraform-labs/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/remote_state_lab.tar.gz
```
```
tar -zxvf remote_state_lab.tar.gz
```
```
cd remote-state-lab
```
```
ls
```
```
cat instance.tf
```
```
cat vars.tf
```
```
cat provider.tf
```
Create a File for storing "**terraform.tfstate**" file in backend. ie.. **Amazon S3.** 

```
vi backend.tf
```
Add the given lines, by pressing "INSERT" 
```
terraform {
  backend "s3" {
    bucket = "<Replace your s3 bucket name>"
    key    = "terraform/remotestate"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
cat backend.tf
```
```
terraform init
```
When it prompts for Region confirmation, enter the same Region in which bucket is created.
```
us-east-2
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply
```
* Go to the S3 bucket and open **terraform** > **remotestate** > In Properties Copy the **Object URL** and paste it in Browser.
  (By default it shows Access Denied)
* To view, Click on permission and click on **Edit** under **Access control list (ACL)** > **Everyone (public access)** > Check "Read" then check **I understand the effects of these changes on this object** and then Click on Save changes
* Copy the **object URL** and paste it into the web browser (If already done, just Refresh the page).
* Now, You should be able to access the state file and View the resources.
  (It shows the attributes of a single resource in the Terraform state of **aws_instance.terraform-remoteState**.)

Use the "terraform destroy" command for cleaning the infrastructure used in this lab, 
```
terraform destroy
```
```
cd ..
```
* remove the directory using "**rm -rf**"
```
rm -rf remote-state-lab
```
Empty the Bucket and then Delete the Bucket.

## Lab-5: Launching VPC and EC2 Instance 

#### Task-1: Launching VPC and creating subnets
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/lab_10_vpc_v0.13.tar.gz
```
```
tar -xvf lab_10_vpc_v0.13.tar.gz
```
```
cd lab_10_vpc/
ls
```
```
cat vpc.tf
```
```
cat nat.tf
```
```
cat vars.tf
```
```
terraform init
```
```
terraform plan
```
**Note:** If it is throwing any Error then either **Comment** it by Adding # in the begining of line or **Remove** the line.

(**Example Error:** on vpc.tf line 12, in resource "aws_vpc" "main": 12:   enable_classiclink   = "false")
```
terraform apply -auto-approve
```
#### Task-2: Launching an EC2 Instance 

Create a new File for EC2 Instance
```
vi instance.tf 
```
Add the given lines, by pressing "INSERT" 
```
resource "aws_instance" "example" {
  ami           = var.AMIS[var.AWS_REGION]
  instance_type = "t2.micro"
  # the VPC subnet
  subnet_id = aws_subnet.main-public-1.id
  # the security group
  vpc_security_group_ids = [aws_security_group.allow-ssh.id]
  # the public SSH key
  key_name = aws_key_pair.mykeypair.key_name
  tags = {
    Name = "Yourname-Lab11-ec2"
  }
}
```
save the file using "ESCAPE + :wq!"

Also, Create a new File for EC2 SecurityGroup
```
vi securitygroup.tf
```
Add the given lines, by pressing "INSERT" 
```
resource "aws_security_group" "allow-ssh" {
  vpc_id      = aws_vpc.main.id
  name        = "yourname-allow-ssh"
  description = "security group that allows ssh and all egress traffic"
  egress {
    from_port   = 0
    to_port     = 0
    protocol    = "-1"
    cidr_blocks = ["0.0.0.0/0"]
  }
  ingress {
    from_port   = 22
    to_port     = 22
    protocol    = "tcp"
    cidr_blocks = ["0.0.0.0/0"]
  }
  tags = {
    Name = "yourname-allow-ssh"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
ssh-keygen -f mykey
```
```
ls
```
```
vi key.tf
```
Add the given lines, by pressing "INSERT" 
```
resource "aws_key_pair" "mykeypair" {
  key_name   = "yourname-keypair"
  public_key = file(var.PATH_TO_PUBLIC_KEY)
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
Add the given lines, by pressing "INSERT" 
##### First 3 lines are already present. you can add the remaining lines.
```
variable "AWS_REGION" {
  default = "us-east-2"
}

variable "PATH_TO_PUBLIC_KEY" {
  default = "mykey.pub"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-2 = "ami-059d836af932792c3"
    us-west-2 = "ami-0a7d051a1c4b54f65"
    eu-west-1 = "ami-04c58523038d79132"
  }
}
```
Save the file using "ESCAPE + :wq!"
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply -auto-approve
```
See details of the specific resource
```
terraform state show aws_instance.example | grep public_ip
```
```
ssh -i mykey -l ubuntu <Your Public IP>
```
exit

#### Task-3: Connecting an EBS with EC2 Instance 
```
vi instance.tf 
```
Add the given lines, by pressing "INSERT" 
First block will be already present in the file
```
resource "aws_instance" "example" {
  ami = var.AMIS[var.AWS_REGION]
  instance_type = "t2.micro"
  # the VPC subnet
  subnet_id = aws_subnet.main-public-1.id
  # the security group
  vpc_security_group_ids = [aws_security_group.allow-ssh.id]
  # the public SSH key
  key_name = aws_key_pair.mykeypair.key_name
  tags = {
    Name = "yourname-Lab11-EC2"
  }
}

resource "aws_ebs_volume" "ebs-volume-1" {
 availability_zone = "us-east-2a"
 size = 20
 type = "gp2"
 tags = {
   Name = "Yourname extra volume"
 }
}

resource "aws_volume_attachment" "ebs-volume-1-attachment" {
  device_name = "/dev/xvdh"
  volume_id = aws_ebs_volume.ebs-volume-1.id
  instance_id = aws_instance.example.id
} 
```
Save the file using "ESCAPE + :wq!"
```
terraform apply
```
* Go to EC2 dashboard and select the EC2 Instance. 
* Then select storage and see that the new volume is attached
```
ssh -i mykey ubuntu@<PUBLIC IP>
```
exit

Use the "terraform destroy" command for cleaning the infrastructure used in this lab, remove the directory using rm -rf
```
terraform destroy -auto-approve
```
```
cd ..
rm -rf lab_10_vpc
```
## Lab-6: Launching Auto-Scaling services

#### Task-1: Create ASG
```
cd /home/ubuntu/
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/lab_14_autoscaling.tar.gz
```
```
tar -zxvf lab_14_autoscaling.tar.gz
```
```
cd lab_14_autoscaling/
```
```
ls
```
```
vi autoscaling.tf
```
Update the names/tags to include your name
```
vi autoscalingpolicy.tf
```
Update the names/tags to include your name
```
ssh-keygen -f mykey
ls
```
```
terraform init
```
```
terraform plan
```
```
terraform apply 
```
#### Task-2: Increase CPU utilization in your new ec2 instance.
```
ssh -i mykey -l ubuntu <Public IP> 
```
```
sudo apt-get update
```
```
sudo apt-get install stress
```
```
stress --cpu 3 -v --timeout 300
```
* Now go back to AWS console > Services > EC2 > Instances and select your instance > Description and click on Monitoring. Now you see High CPU utilization (Or)
* Go to AWS console > Services > CloudWatch > Alarms and view alarm
* To check whether the new Instance is added, Go to AWS console > Services > EC2 > Instances. You can see that one more instance is created with the same name.  

Once done **Exit** from EC2 Instance
```
terraform destroy
```

## Lab-7: Creating a MySQL Database with RDS 
```
cd /home/ubuntu
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/lab_12_rds.tar.gz
```
```
tar -zxvf lab_12_rds.tar.gz
```
```
cd lab_12_rds
```
```
ls
```
```
cat rds.tf
```
```
cat output.tf
```
Change the tags to include your name. change the key pair name to include your name
```
ssh-keygen -f mykey
ls
```
```
terraform init
```
```
terraform plan
```
**Note:** If you find any error on Line No-24 (**name = "mydb"**) just open file and Comment it by adding #.
```
terraform apply -auto-approve
```
```
ssh -i mykey -l ubuntu <PUBLIC IP>
```
```
sudo apt-get update
```
```
sudo apt-get install mysql-client
```
Get the endpoint from the AWS console. do not include the port number while entering the endpoint
* MySQL -u admin -h **mysql.cfkfveiseeie.us-east-2.rds.amazonaws.com** -p
```
mysql -u admin -h <endpoint> -p
```
* use 'password' as the password.
After LogingIn Use Commands for DataBases
```
show databases;
```
#### Create a new database
```
create database demoDB;
```
```
show databases;
```
```
use demoDB
```
#### Use the 'status' command to list the DB in which you are in.
```
status
```
#### Create table
```
create table customers(
   ID   INT              NOT NULL,
   NAME VARCHAR (20)     NOT NULL,
   AGE  INT              NOT NULL,
   ADDRESS  CHAR (25) ,
   SALARY   DECIMAL (18, 2),       
   PRIMARY KEY (ID)
);
```
#### Use the "desc" command to check if the table is created
```
desc customers;
```
Include some values in the DB:
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (1, 'Richard', 32, 'London', 50000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (2, 'Rachel', 25, 'Paris', 45000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (3, 'Mathew', 23, 'Brussels',55000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (4, 'John', 25, 'Texas', 65000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (5, 'Jane', 27, 'NewYork', 60000.00 );
```
```
INSERT INTO customers (ID,NAME,AGE,ADDRESS,SALARY)
VALUES (6, 'Komal', 22, 'Bangalore', 45000.00 );
```
#### Display the table:
```
SELECT * FROM customers;
```
```
select NAME, AGE from customers;
```
#### To delete the table:
```
drop table customers;
```
* **Exit** from mysql client.
* **Exit** from the mysql **anchor EC2.**
```
terraform destroy
```
## Lab-8: Creating IAM Users, Groups using Terraform.
```
cd /home/ubuntu
```
```
mkdir iam-users && cd iam-users
```
Create a new **iam.tf** file to define your IAM resources.
```
vi iam.tf
```
Copy and paste the below Code to create uers and group.
```
provider "aws" {
  region = "us-east-1"
}

resource "aws_iam_group" "example_group" {
  name = "infosys"
}

resource "aws_iam_user" "example_user" {
  for_each = toset(["user1", "user2", "user3", "user4", "user5", "user6", "user7", "user8", "user9", "user10"])

  name = each.key
}

resource "aws_iam_user_group_membership" "example_membership" {
  for_each = aws_iam_user.example_user

  user   = each.value.name
  groups = [aws_iam_group.example_group.name]
}

```
```
terraform init
```
```
terraform fmt
```
```
terraform plan
```
```
terraform apply
```
Destroy the resources once done.
```
terraform destroy
```
## Lab-9: Creating AWS resources using terraform modules
```
cd /home/ubuntu/
```
```
sudo apt install tree -y
```
```
wget https://s3.ap-south-1.amazonaws.com/files.cloudthat.training/devops/terraform-essentials/terraform-modules.tar.gz
```
```
tar -xvf terraform-modules.tar.gz
```
```
cd terraform-modules
```
```
tree
```
Cat all files to see the module structure
```
vi main.tf
```
Add the below code after block **module "my_security_group"**
```
output "secgrpid" {
  description = "Newly created sec grp"
  value       = module.my_security_group.sgid
}
```
```
cat provider.tf
```
No change needed
```
vi variables.tf 
```
**Note:**

Replace the **VPC ID** & **Subnet ID** from **ca-central-1** region in **variable.tf** file.
* **Change vpc_id** to any VPC in ca-central-1 (Ex: vpc-0e608033e14b01c3c)
* **Change subnet id** - use available subnets from AZ **a or b**. (Ex: subnet-086dd80df2e64b56b)

Save it

Now, Create a key pair. The same public key will be saved into the EC2 being launched.
```
ssh-keygen -f mykey
```
```
terraform init
```
```
terraform fmt
```
```
terraform validate
```
```
terraform plan
```
```
terraform apply -auto-approve
```
**Note:** 
* If it is showing any **Error** for **Security group / KeyPair**, It means that they are already existing, delete the Keypair/Security Group in Console or Rename it in **variable.tf** file.

Once the resources are created. Then, verify all the resources and then destroy them.
```
terraform destroy
```
-----------------------------------------------------------------------------------------------------------------------------
## Frequently used Terraform Commands with Explanation
1. terraform version

**Explanation:** This command displays the version of Terraform currently installed on your system. It's a quick way to check the installed version and make sure it matches the version you intend to use.
```
terraform version
```
2. terraform init

**Explanation:** This command initializes and Prepares your working directory. It downloads the necessary provider plugins and sets up the backend configuration defined in your terraform block.
```
terraform init
```
3. terraform providers

**Explanation:** This command shows a list of installed provider plugins along with their versions. Providers are responsible for managing resources on various cloud platforms.
```
terraform providers
```
4. terraform fmt

**Explanation:** This command automatically formats your Terraform configuration files to follow a consistent style. It helps in maintaining a clean and readable codebase.
```
terraform fmt
```
5. terraform validate

**Explanation:** This command validates the syntax and structure of your Terraform configuration files. It checks for any errors or issues in your code.
```
terraform validate
```
6. terraform plan

**Explanation:** This command generates an execution plan. It compares the current state of your infrastructure with the desired state defined in your Terraform files. It shows you what changes Terraform will make to achieve the desired state without actually making those changes.
```
terraform plan
```
7. terraform apply

**Explanation:** This command applies the changes defined in your Terraform configuration to the infrastructure. It creates, updates, or deletes resources as necessary to match the desired state.
```
terraform apply
```
8. terraform output

**Explanation:** This command displays the values of **output variables** defined in your Terraform configuration. Output variables provide information about the resources created or managed by Terraform.
```
terraform output
```
9. terraform show

**Explanation:** This command displays the current state of your infrastructure as recorded in the Terraform state file. It provides a human-readable representation of the resources that Terraform is managing, along with their attributes and current values.
```
terraform show
```
10. terraform get

**Explanation:** This command is used to download and update the modules specified in your configuration. Modules are reusable components that encapsulate infrastructure resources and their configurations.
```
terraform get
```
11. terraform destroy

**Explanation:** This command is used to destroy the infrastructure defined in your Terraform configuration. It deletes all the resources that were created using Terraform.
```
terraform destroy
```
12. terraform import

**Explanation:** The terraform import command allows you to import existing infrastructure resources into your Terraform state. This is useful when you have resources that were not initially created and managed by Terraform, but you want to start managing them using Terraform moving forward.
```
terraform import
```
13. terraform refresh

**Example:** Let's say you have an AWS EC2 instance defined in your configuration. If someone manually stops or starts the instance using the AWS Management Console, Terraform wouldn't be aware of this change until you run terraform refresh. Once you do, Terraform will update its state to match the current status of the instance.
```
terraform refresh
```
14. terraform login

**Explanation:** This command is used to obtain and save credentials for a remote host, typically used with remote backends.
```
terraform login
```
15. terraform logout

**Explanation:** This command removes locally-stored credentials for a remote host.
```
terraform logout
```
16. terraform taint

*terraform taint aws_instance.example*

**Explanation:** This command marks a resource instance as not fully functional. It's used to indicate that a resource has issues and needs to be recreated.
```
terraform taint
```
17. terraform untaint

*terraform untaint aws_instance.example*

**Explanation:** This command removes the 'tainted' state from a resource instance, indicating that the resource is now functional.
```
terraform untaint
```
18. terraform workspace

*terraform workspace new dev* (This would create a new workspace named "dev.")

**Explanation:** This category of commands is used for workspace management. Workspaces allow you to manage multiple environments (such as development, staging, and production) within the same configuration.
```
terraform workspace
```
## Reference Links
1. [What is Terraform?](https://developer.hashicorp.com/terraform/intro)
2. [Terraform Providers](https://registry.terraform.io/browse/providers)
3. [Install Terraform](https://developer.hashicorp.com/terraform/downloads)
4. [Get Started - AWS](https://developer.hashicorp.com/terraform/tutorials/aws-get-started)
5. [AWS Free Tier](https://aws.amazon.com/free/?all-free-tier.sort-by=item.additionalFields.SortRank&all-free-tier.sort-order=asc&awsf.Free%20Tier%20Types=*all&awsf.Free%20Tier%20Categories=*all)
6. [AWS Instance Sample Code - Resource: aws_instance](https://registry.terraform.io/providers/hashicorp/aws/latest/docs/resources/instance)
7. [Variables and Outputs](https://developer.hashicorp.com/terraform/language/values)
8. [Terraform Modules](https://developer.hashicorp.com/terraform/language/modules)
9. [Terraform Tutorials](https://developer.hashicorp.com/terraform/tutorials)
10. [Prepare for Terraform Certification](https://developer.hashicorp.com/terraform/tutorials/certification-003)
11. [Terraform Beginner FAQs and Examples](https://www.hashicorp.com/resources/solutions-eng-webinar-episode-1-terraform)
12. [hashicorp/terraform-GitHub guides](https://github.com/hashicorp/terraform-guides/tree/master/infrastructure-as-code)
