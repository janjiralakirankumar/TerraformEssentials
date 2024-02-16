# Terraform Essentials Lab Cheat Sheet

### Terraform Labs Pre-requisites
1. Basic understanding of Linux Commands.
2. Basic knowledge of a Cloud platform such as AWS.
3. Good to have an AWS-Free Tier Account for Practice.
4. Copy and paste a few details of your Allocated Region to a text file that we frequently use in labs as mentioned below:
     - Region (**EX:** us-east-1)
     - Availability Zones (**EX:** us-east-1a, us-east-1b)
     - AMI IDs (**EX:** Amazon Linux, RedHat, Ubuntu)
     - VPC ID, Subnet ID, Security group ID, KeyPair Name.

## Table Of Contents
* [Lab-1: Creating an EC2 Instance in AWS and Installing Terraform](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-1-creating-an-ec2-instance-in-aws-and-installing-terraform)
* [Lab-2: AWS EC2 instance creation using Terraform Variables](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-2-aws-ec2-instance-creation-using-terraform-variables)
* [Lab-3 : Using Output Feature](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-3--using-output-feature)
* [Lab-4 : Remote State using Amazon Simple Storage Service](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-4--remote-state-using-amazon-simple-storage-service)
* [Lab-5: Launching VPC and EC2 Instance](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-5-launching-vpc-and-ec2-instance)
* [Lab-6: Launching Auto-Scaling services](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-6-launching-auto-scaling-services)
* [Lab-7: Creating a MySQL Database with RDS](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-7-creating-a-mysql-database-with-rds)
* [Lab-8: Creating IAM Users, Groups using Terraform.](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-8-creating-iam-users-groups-using-terraform)
* [Lab-9: Creating AWS resources using terraform modules](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#lab-9-creating-aws-resources-using-terraform-modules)
* [Frequently used Terraform Commands with Explanation](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#frequently-used-terraform-commands-with-explanation)
* [Reference Links](https://github.com/janjiralakirankumar/TerraformEssentials?tab=readme-ov-file#reference-links)

## Lab-1: Creating an EC2 Instance in AWS and Installing Terraform

To begin with Lab-1, log in to AWS Console.

### Task-1: Installing Terraform on Ubuntu 20.04 operating system

* Manually Launch a `t2.micro` instance with OS version as `Ubuntu 22.04 LTS` in North Virginia (us-east-1) Region.
* Use tag "`Name:Terraform-Server`"
* Create a new Keypair with the Name `Terraform-Keypair-YourName`
* In security groups, include ports `22 (SSH)` and `80 (HTTP)`.
* Configure Storage: 10 GiB
* Launch the Instance.
* Once Launched, Connect to the Instance using `MobaXterm` or `Putty` with username "`ubuntu`".

Once the EC2 is ready, follow the below Commands to perform lab:
```
sudo hostnamectl set-hostname TerraformServer
bash
```
```
sudo apt update
```
```
sudo apt install wget unzip -y
```
```
wget https://releases.hashicorp.com/terraform/1.7.3/terraform_1.7.3_linux_amd64.zip
```
To know the latest Terraform version - [Install Terraform](https://developer.hashicorp.com/terraform/downloads)
```
unzip terraform_1.7.3_linux_amd64.zip
```
```
ls
sudo mv terraform /usr/local/bin
```
```
rm terraform_1.7.3_linux_amd64.zip
```
```
ls
terraform
```
```
terraform -v
```

### Task-2: Install Required Packages and login to Ubuntu server using Credentials. 
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
  
| `Access Key ID.` | `Secret Access Key ID.` |
| ------------------ | ------------------------- |
| AKIAIOSFODNN7EXAMPLE | wJalrXUtnFEMI/K7MDENG/bPxRfiCYEXAMPLEKEY |

##### Note: If Credentials are not available generate from AWS IAM Service
Once LoggedIn check the account access
```
aws s3 ls
```
`Or` Use below command to check whether it is authenticated.
```
aws iam list-users
```
### Task-3: Now we are ready to perform the labs
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
To check whether the file is created follow the command

```
cd /home/ubuntu
ls
```
```
cat test.txt
```
Once Verified, you can destroy the File by changing it to the Lab1 Directory
```
cd Lab1
```
```
terraform destroy
```
```
cd ~
rm -rf Lab1
```
### Task-4: Launching your first AWS EC2 instance using Terraform 
```
cd ~
mkdir EC2-lab && cd EC2-lab/
```
```
vi example.tf
```
Add the given lines, by pressing "INSERT"

##### Note: 
1. Replace your allocated `Region` and `AMI ID` of the same Region.
2. `"Yourname-EC2-1"` with Your Name.
```
provider "aws" {
  profile = "default" # This line is not mandatory.
  region  = "us-east-2"
}

resource "aws_instance" "example" {
  ami           = "ami-07efac79022b86107"
  instance_type = "t2.micro"
  tags = {
    Name = "Yourname-EC2-1"
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
#List the files
ls 
```
To see what is saved in `terraform.tfstate` use the below command.

```
cat terraform.tfstate
```
### Task-5: Now, Let's Replace the AMI ID with `Amazon Linux AMI 2023` from the same region and see what happens.

`Note:` To get the `Amazon Linux AMI 2023` go to EC2 Instances, click on Launch, and select the `Amazon Linux AMI 2023` AMI and copy the ID and paste in File.
```
vi example.tf
```
Replace the AMI ID, by pressing "INSERT"  
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

Now in Terraform Plan Verify the Changes.
```
terraform plan
```
```
terraform apply
```
Once Applied then go to the console and see that the Previous EC2 is being destroyed and new EC2 is getting created.
    
Also verify that the OS is `Amazon Linux AMI 2023`

Again when you cat the `terraform.tfstate` file it shows a new AMI ID.
```
cat terraform.tfstate
```
Use the `terraform destroy` command for cleaning the infrastructure used in this lab
```
terraform destroy
```
Finally, verify that the resources are deleted in the Console.

#### =========================END of LAB-01=========================

## Lab 2: AWS EC2 instance creation using Terraform Variables


### Task-1: Create EC2 instance using variables 
```
cd /home/ubuntu/EC2-lab/
```
```
mkdir variables-lab && cd variables-lab/
```
Now Create four Files ie. `provider.tf,` `vars.tf,` `terraform.tfvars,` `instance.tf.`
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
Add the given lines, by pressing "INSERT" also replace your `Region`
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
Add the given lines, by pressing "INSERT" Also replace them with your `AccessKey` and `Secret Access Keys` that you created and downloaded earlier.
```
AWS_ACCESS_KEY="< Insert your AWS Access Key >"
AWS_SECRET_KEY="< Insert your AWS Secret Key >"
```
Save the file using "ESCAPE + :wq!"
```
vi instance.tf
```
Add the given lines, by pressing "INSERT" Also replace your `region's AMI ID` and `YourName`
```
resource "aws_instance" "terraform_example"{
  ami = "ami-07efac79022b86107"
  instance_type="t2.micro"
  tags = {
    Name = "Variables-Lab2-YourName"
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
Once applied, go to the Console and check that the new EC2 is created using Variables.

Use the `"terraform destroy"` command to clean the infrastructure used in this lab
```
terraform destroy -auto-approve
```

### Task-2: Implementing map variables that dynamically fetch AMI based on the Linux distro selected
```
vi instance.tf
```
Delete all the existing lines and Add the given code, by pressing "INSERT" and replacing `YourName`

`Note:` To delete all the lines at a time use the commands `Esc+gg+dG` and once cleared then add the new lines.
```
resource "aws_instance" "terraform_example"{
  ami = var.AMIS[var.Linux_distro]
  instance_type="t2.micro"
  tags = {
    Name = "YourName-Lab2-Task2"
  }
}
```
Save the file using "ESCAPE + :wq!"

Now, Also make changes in the `Vars.tf` file.
```
vi vars.tf
```
Delete all existing lines and Add the given lines, by pressing "INSERT" Also ensure to replace your `Region,` `AMi IDs` of the same region for `redhat,` `ubuntu` and `amazon`

`Note:` To delete all the lines at a time use `Esc+gg+dG`
```
variable "AWS_ACCESS_KEY"{}
variable "AWS_SECRET_KEY"{}
variable "AWS_REGION"{
  default = "us-east-2"
  }

variable "Linux_distro"{
  #description = "Please Enter the Linux distro (redhat, ubuntu, amazon)"
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
Use the `terraform destroy` command to clean the infrastructure used in this lab
```
terraform destroy
```
Once Done remove the `EC2-lab` Directory.
```
cd ~
rm -rf EC2-lab
```
#### =========================END of LAB-02=========================

## Lab-3 : Using Output Feature 

### Task-1: Using output feature of Terraform to get the IP Address of EC2 Instance
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
In `vars.tf` file delete all the lines and add the below code. Also, ensure to replace your `region` and Include your `region's Ubuntu AMI ID` to the list.
```
vi vars.tf
```
```
variable "AWS_REGION" {
  default = "us-east-1"
}

variable "AMIS" {
  type = map(string)
  default = {
    us-east-2 = "ami-089c26792dcb1fbd4"
    us-west-2 = "ami-00448a337adc93c05"
    eu-west-1 = "ami-0e309a5f3a6dd97ea"
  }
}
```
Once replaced, save the changes.
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
Use the `terraform destroy` command to clean the infrastructure used in this lab.
```
terraform destroy
```
Once done, remove the directory and Zip file using the `rm -rf` command below.
```
cd ~
rm -rf output-variable-lab
```
```
rm -rf output-variable-lab-v0.13.5.tar.gz
```
#### =========================END of LAB-03=========================

## Lab-4 : Remote State using Amazon Simple Storage Service 

### Task-1: Manually Create a S3 Bucket using AWS Console 

* Create a new S3 bucket in your Allocated Region by name: `youryame-terraform` (S3 allows lowercase only)
* While creating,
    - Select `"ACLs enabled"`
    - Uncheck `"block public access"` and select `"I acknowledge that the current settings might result in this bucket and the objects within becoming public"`
    - `"Enable versioning"`
    - Then, click on `Create bucket`
* Once done, To cross-check whether the Bucket is created or not, run the below command in CLI.
```
aws s3 ls 
```
### Task-2: Configure Remote State
```
cd ~
mkdir S3-Lab && cd S3-Lab
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
vi vars.tf
```
In `vars.tf` file ensure to replace your `region` and Include your `region's Ubuntu AMI ID` to the list.

Once done, save the file and follow further steps.
```
cat provider.tf
```
Now, Create a New Configuration File for storing "`terraform.tfstate`" file in the backend. (ie. `Amazon S3.`)

```
vi backend.tf
```
Add the given lines, by pressing "INSERT" and add your `bucket's region` and `Bucket Name`
```
terraform {
  backend "s3" {
    region = "<Replace your s3 bucket region>"
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
* Go to the S3 bucket and click on `terraform` > `remotestate` > In Properties Copy the `Object URL` and paste it in Browser.
  (By default it shows Access Denied)
* To view the content of the file, in S3 Bucket tab, Click on `permission` and click on `Edit` under `Access control list (ACL)` > `Everyone (public access)` > Check `"Read"` then check `I understand the effects of these changes on this object` and then Click on `Save changes`
* Refresh the Object URL Page in the browser (or again Copy-paste the `object URL` into the web browser).
* Now, You should be able to access the state file and View the resources.
  (It shows the attributes of a single resource in the Terraform state of `aws_instance.terraform-remoteState`.)

Use the `terraform destroy` command to clean the infrastructure used in this lab, 
```
terraform destroy
```
Once done, Remove the directory and Zip file using "`rm -rf`"
```
cd ~
rm -rf S3-Lab
```
**Note:** Also Ensure to delete the `S3 Bucket` (To delete, first empty the Bucket and then Delete it.)

#### =========================END of LAB-04=========================

## Lab-5: Launching VPC and EC2 Instance 

### Task-1: Launching VPC and creating subnets
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
Now, Open the files one by one and replace your regions (**Ex:** ap-south-1) and for Availability Zones (**Ex:** ap-south-1a)
```
vi vpc.tf
```
In `vpc.tf` file Add `#` in front of line 12, ie... `enable_classiclink = "false"` and replace your `AZs`
```
cat nat.tf
```
No Updated need in `nat.tf`
```
vi vars.tf
```
In `vars.tf` file Update your Region.
```
terraform init
```
```
terraform plan
```
**Note:** In `terraform plan` if it shows any warning just ignore it. But, if it shows any error we need to correct it.

```
terraform apply -auto-approve
```
Once applied, In the Console check that the resources are getting created like `VPC,` `Subnets,` `Internet Gateway` etc....

### Task-2: Launching an EC2 Instance 

Create a `new File` for EC2 Instance
```
vi instance.tf 
```
Add the given lines, by pressing "INSERT" and replace `Yourname-Lab5-ec2` with your name.
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
    Name = "Yourname-Lab5-ec2"
  }
}
```
save the file using "ESCAPE + :wq!"

Also, Create a new File for EC2 SecurityGroup
```
vi securitygroup.tf
```
Add the given lines, by pressing "INSERT" and replacing your name at `YourName-allow-ssh`
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
    Name = "YourName-allow-ssh"
  }
}
```
Save the file using "ESCAPE + :wq!"

Now, Create a new keypair that we use for our EC2 Instance.
```
ssh-keygen -f mykey
```
```
ls
```
```
vi key.tf
```
Add the given lines, by pressing "INSERT" and replace with `your name`.
```
resource "aws_key_pair" "mykeypair" {
  key_name   = "YourName-KeyPair"
  public_key = file(var.PATH_TO_PUBLIC_KEY)
}
```
Save the file using "ESCAPE + :wq!"
```
vi vars.tf
```
Add the given lines, by pressing "INSERT" and replace your `region` and add your `Region's AMI` to the list.

##### The first 3 lines are already present. you can add the remaining lines.
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
Once applied, verify that the `EC2 Instance` is created in the `Custom VPC` that we created in Task-1.

Also, to See the `Public IP` of the newly created EC2 Instance use the below command.
```
terraform state show aws_instance.example | grep public_ip
```
To `SSH` into newly launched EC2, run below command.
```
ssh -i mykey -l ubuntu <Your Public IP>
```
Once verified, `Exit` from Instance.

### Task-3: Connecting an EBS with EC2 Instance 
```
vi instance.tf 
```
Add the given lines, by pressing "INSERT" The first block will be already present in the file. But, replace the entire code and update your `AvailabilityZone,` `YourName` in `AWS Instance` and `EBS Volume` both.
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
    Name = "YourName-Lab5-EC2"
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
* Go to `EC2 Dashboard` and select the `EC2 Instance.` 
* Then select `storage` and see that the new volume is attached.

Once Done, Use the `terraform destroy` command to clean the infrastructure used in this lab.
```
terraform destroy -auto-approve
```
**Note:** Ensure to crosscheck that all the resources are destroyed or else it will charge you. 

Remove the directory and Zip file using `rm -rf`
```
cd ..
rm -rf lab_10_vpc
```
```
rm -rf lab_10_vpc_v0.13.tar.gz
```
#### =========================END of LAB-05=========================

## Lab-6: Launching Auto-Scaling services

### Task-1: Create ASG
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
ls
```
```
cat autoscaling.tf
```
```
cat autoscalingpolicy.tf
```
```
vi vpc.tf
```
In `vpc.tf` file Add `#` in front of line 7, ie... `enable_classiclink = "false"` and replace your `AZs` for `3 public Subnets` and `3 Private Subnets`
```
vi vars.tf
```
In `vars.tf` file replace your `region` and Include your `region's AMI ID` in the list.
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
Now, Check In the Console that the resources are created 

  1. `EC2 dashboard` >> `Auto Scaling Groups` >> `example-autoscaling`
  2. `Amazon Cloud Watch` >> `Alarms`
  3. `VPC,` `Subnets,` and `RT` etc...


### Task-2: Increase CPU utilization in your new ec2 instance.

Now, let's connect with the EC2 created by ASG.

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
* Now go back to `AWS console` > `EC2 Dashboard` > `Instances` then select the New Instance Created by `ASG` > `Description` and click on `Monitoring.` Now you see High CPU utilization

  (Or)
  
* Go to `AWS Console` > `CloudWatch` > `Alarms` and observe the alarm states and CPU Utilization.
* To check whether the `New Instance` is added, Go to `AWS Console` > `EC2` > `Instances.` You can see that one more instance is created with the same name because of the high CPU.  

Once done `Exit` from EC2 Instance
```
terraform destroy
```
Once done, remove the Directory and Zip file.
```
cd ~
rm -rf lab_14_autoscaling
```
```
rm -rf lab_14_autoscaling.tar.gz
```
#### =========================END of LAB-06=========================

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
vi rds.tf
```
In `rds.tf` file in line-24 comment the line by adding #.
```
cat output.tf
```
```
vi vpc.tf
```
In `vpc.tf` file Add `#` in front of line 7, ie... `enable_classiclink = "false"` and replace your `AZs` for `3 Public Subnets` and `3 Private Subnets`
```
vi vars.tf
```
In `vars.tf` file replace your `region` and add your `region's AMI ID` in the list.
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
`Note:` If you find any error on Line No-24 (`name = "mydb"`) just open file and Comment it by adding #.
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
Get the endpoint from the AWS console. (do not include the port number while entering the endpoint)

**Example:** MySQL -u admin -h `mysql.cfkfveiseeie.us-east-2.rds.amazonaws.com` -p
```
mysql -u admin -h <endpoint> -p
```
`Note:` When it prompts for the password enter `password`

After LogingIn Use below Commands for DataBases
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
#### Use the `status` command to list the DB in which you are in.
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
#### To view the table:
```
SELECT * FROM customers;
```
To view only selected attributes, run below command.
```
select NAME, AGE from customers;
```
#### To delete the table:
```
drop table customers;
```
* `Exit` from mysql client.
* `Exit` from the mysql `anchor EC2.`
```
terraform destroy
```
Once destroyed, Delete the directory and zip file as well.
```
cd ~
rm -rf lab_12_rds
```
```
rm -rf lab_12_rds.tar.gz
```
#### =========================END of LAB-07=========================

## Lab-8: Creating IAM Users, Groups using Terraform.
```
cd /home/ubuntu
```
```
mkdir iam-users && cd iam-users
```
Create a new `iam.tf` file to define your IAM resources.
```
vi iam.tf
```
Copy and paste the below Code to create users and group also replace your `region`
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
Once applied, verify the IAM User and group in the console and then Destroy.
```
terraform destroy
```
Once destroyed, remove the Directory.
```
cd ~
rm -rf iam-users
```
#### =========================END of LAB-08=========================

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
Add the below code after block `module "my_security_group"`
```
output "secgrpid" {
  description = "Newly created sec grp"
  value       = module.my_security_group.sgid
}
```
```
cat provider.tf
```
**Note:** No change needed in `provider.tf`
```
vi variables.tf 
```
**Note:** Replace the `Region` Default `VPC ID,` `AMI Id` and `Subnet ID` from your Allocated region in `variable.tf` file.
* `Change vpc_id` to default VPC in your region (**Ex:** vpc-0e608033e14b01c3c)
* `Change subnet id` Use any available subnets from AZ `a or b`. (**Ex:** subnet-086dd80df2e64b56b)

Then, Save it

Now, Create a key pair. The same public key will be used in the new EC2 Instance.
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
* If it is showing any `Error` for `Security group / KeyPair`, It means that they are already existing, Rename it in `variable.tf` file or delete the Keypair/Security Group in the Console.

Once the resources are created. Then, verify all the resources and then destroy them.
```
terraform destroy
```
Once Destroyed, remove the Directory and Zip FIle.
```
cd ~
rm -rf terraform-modules
```
```
rm -rf terraform-modules.tar.gz
```
#### =========================END of LAB-09=========================
-----------------------------------------------------------------------------------------------------------------------------
## Frequently used Terraform Commands with Explanation

#### 1. terraform version

`Explanation:` This command displays the version of Terraform currently installed on your system. It's a quick way to check the installed version and make sure it matches the version you intend to use.
```
terraform version
```
#### 2. terraform init

`Explanation:` This command initializes and Prepares your working directory. It downloads the necessary provider plugins and sets up the backend configuration defined in your terraform block.
```
terraform init
```
#### 3. terraform providers

`Explanation:` This command shows a list of installed provider plugins along with their versions. Providers are responsible for managing resources on various cloud platforms.
```
terraform providers
```
#### 4. terraform fmt

`Explanation:` This command automatically formats your Terraform configuration files to follow a consistent style. It helps in maintaining a clean and readable codebase.
```
terraform fmt
```
#### 5. terraform validate

`Explanation:` This command validates the syntax and structure of your Terraform configuration files. It checks for any errors or issues in your code.
```
terraform validate
```
#### 6. terraform plan

`Explanation:` This command generates an execution plan. It compares the current state of your infrastructure with the desired state defined in your Terraform files. It shows you what changes Terraform will make to achieve the desired state without actually making those changes.
```
terraform plan
```
#### 7. terraform apply

`Explanation:` This command applies the changes defined in your Terraform configuration to the infrastructure. It creates, updates, or deletes resources as necessary to match the desired state.
```
terraform apply
```
#### 8. terraform output

`Explanation:` This command displays the values of `output variables` defined in your Terraform configuration. Output variables provide information about the resources created or managed by Terraform.
```
terraform output
```
#### 9. terraform show

`Explanation:` This command displays the current state of your infrastructure as recorded in the Terraform state file. It provides a human-readable representation of the resources that Terraform is managing, along with their attributes and current values.
```
terraform show
```
#### 10. terraform get

`Explanation:` This command is used to download and update the modules specified in your configuration. Modules are reusable components that encapsulate infrastructure resources and their configurations.
```
terraform get
```
#### 11. terraform destroy

`Explanation:` This command is used to destroy the infrastructure defined in your Terraform configuration. It deletes all the resources that were created using Terraform.
```
terraform destroy
```
#### 12. terraform import

`Explanation:` The terraform import command allows you to import existing infrastructure resources into your Terraform state. This is useful when you have resources that were not initially created and managed by Terraform, but you want to start managing them using Terraform moving forward.
```
terraform import
```
#### 13. terraform refresh

`Example:` Let's say you have an AWS EC2 instance defined in your configuration. If someone manually stops or starts the instance using the AWS Management Console, Terraform wouldn't be aware of this change until you run terraform refresh. Once you do, Terraform will update its state to match the current status of the instance.
```
terraform refresh
```
#### 14. terraform login

`Explanation:` This command is used to obtain and save credentials for a remote host, typically used with remote backends.
```
terraform login
```
#### 15. terraform logout

`Explanation:` This command removes locally-stored credentials for a remote host.
```
terraform logout
```
#### 16. terraform taint

*terraform taint aws_instance.example*

`Explanation:` This command marks a resource instance as not fully functional. It's used to indicate that a resource has issues and needs to be recreated.
```
terraform taint
```
#### 17. terraform untaint

*terraform untaint aws_instance.example*

`Explanation:` This command removes the 'tainted' state from a resource instance, indicating that the resource is now functional.
```
terraform untaint
```
#### 18. terraform workspace

*terraform workspace new dev* (This would create a new workspace named "dev.")

`Explanation:` This category of commands is used for workspace management. Workspaces allow you to manage multiple environments (such as development, staging, and production) within the same configuration.
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
---
### Summaries of each lab in the Terraform Essentials Cheat Sheet:

**Lab-1: Creating an EC2 Instance in AWS and Installing Terraform**
- Manually create an EC2 instance in AWS.
- Install Terraform on the EC2 instance.
- Launch an EC2 instance with specific configurations.
- Connect to the EC2 instance securely.
- Create a basic local Terraform configuration to understand Terraform workflow.
- Perform resource cleanup by destroying the created resources.

**Lab-2: AWS EC2 instance creation using Terraform Variables**
- Use Terraform variables to create an EC2 instance.
- Create a Terraform configuration with variables for AWS access keys, secret keys, and the region.
- Create a map variable that dynamically selects the AMI based on the Linux distribution.
- Gain insights into using variables in Terraform configurations.

**Lab-3: Using Output Feature**
- Demonstrate the use of the Terraform output feature.
- Create an EC2 instance and retrieve its public and private IP addresses using Terraform outputs.
- Learn how to extract and display data from Terraform-managed resources.

**Lab-4: Remote State using Amazon Simple Storage Service (S3)**
- Configure Terraform to store its state file in Amazon S3 as a remote backend.
- Manually create an S3 bucket and configure Terraform to use it for state file storage.
- Emphasize the importance of remote state for collaboration and multi-machine workflows.
- Learn about securing S3 buckets and managing access to them.

**Lab-5: Launching VPC and EC2 Instance**
- Launch a Virtual Private Cloud (VPC) and create subnets using Terraform.
- Configure the VPC, subnets, security groups, and an EC2 key pair.
- Gain hands-on experience in provisioning infrastructure on AWS using Terraform.

**Lab-6: Launching Auto-Scaling Services**
- Create an Auto Scaling Group (ASG) using Terraform.
- Enable dynamic management of a group of EC2 instances based on traffic demand.
- Create CloudWatch alarms to monitor the ASG's performance.
- Simulate increased CPU utilization to observe automatic scaling.
- Understand how to scale infrastructure automatically in response to varying workloads.

**Lab-7: Creating a MySQL Database with RDS**
- Guide on creating an Amazon RDS (Relational Database Service) instance for MySQL using Terraform.
- Set up the RDS instance, connect to it, and perform basic database operations.
- Learn how to use Terraform to provision managed database services on AWS.

**Lab-8: Creating IAM Users and Groups using Terraform**
- Use Terraform to create IAM (Identity and Access Management) users and groups on AWS.
- Define IAM resources, including groups and users.
- Associate users with groups for access control.
- Gain practical experience in managing access to AWS resources using Terraform.

**Lab-9: Creating AWS Resources using Terraform Modules**
- Demonstrate the use of Terraform modules for provisioning AWS resources.
- Deploy resources like VPC, security groups, EC2 instances, and subnets using pre-built modules.
- Simplify infrastructure provisioning by encapsulating resources into reusable modules.
- Focus on module structuring and creating resources efficiently.
