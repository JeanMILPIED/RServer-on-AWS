# RServer-on-AWS
instructions to build a RServer on AWS instance

## description of the R-Server structure
* the R server will be open on a public instance on your public VPC on AWS
* to avoir money cost, you will use a free tier EC2 instance on AWS with Centos Linux v6
* please take great care of the OS version used on your instance, and the related R-Server version
* any detailed documentation can be found on 
  * https://rstudio.com/products/rstudio/download-server/redhat-centos/
  * https://docs.rstudio.com/resources/install-r/


## Option1: create an instance and manually install the R-Server on it
### steps to built your VPC and public instance to "hold" the R-Server
* create a VPC with private IP like 192.168.0.0/16: named JEAN_VPC
* create a public Subnet with private IP like 192.168.1.0/24: named JEAN_PUBLIC
* associate an Internet Gateway to your VPC: named JEAN_IGW
* open an EC2 instance in your Public Subnet: named R-Server instance
 * AWS/Services/EC2/Launch Instance
 * Amazon Linux 2 AMI (HVM), SSD Volume Type 
 * Instance type: General Purpose / T2.Micro / Free Tier
 * 1 instance / associated to JEAN_VPC / Associated to public subnet JEAN_PUBLIC / Enable auto-assign IP
 * Add storage: OK (no specific option to add)
 * AddTags / AddTag/ tag = Name; Value = R-Server1;
 * configure security group: NEW / SG_RSERVER / Type: SSH; Protocol: TCP; POrt: 22; SOurce: myIP
 * then LAUNCH instance and save key_pair on your hard drive at a known folder
 

### how to install the R server on your instance
* connect to your EC2 instance with SSH protocole from your computer using the console code:
`ssh -i "JM_Keypair.pem" ec2-user@3.234.223.11`  
(note: don't forget to place yourself in the directory where your keypair is with the `cd` command)  
* update your instance with linux code (and wait for the update):
`sudo yum update -y`
*

## Option2: create an instance and add code into it for automatic install of the R-Server on it
### follow the same steps as Option1 but:
* in the Advanced Details section of the Instance creation panel, add:  
`#!/bin/bash`  
`sudo yum update -y`  
`sudo amazon-linux-extras install R3.4 -y`  
`wget https://download2.rstudio.org/server/centos6/x86_64/rstudio-server-rhel-1.2.5019-x86_64.rpm`  
`sudo yum -y install rstudio-server-rhel-1.2.5019-x86_64.rpm`  
`sudo useradd jean`  
`echo jean:1234 | sudo chpasswd`  

### how to connect to your R server on your instance with username and password
