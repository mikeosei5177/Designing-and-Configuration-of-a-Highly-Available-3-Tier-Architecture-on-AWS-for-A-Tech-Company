# Designing-and-Configuration-of-a-Highly-Available-3-Tier-Architecture-on-AWS-for-A-Tech-Company

Designing and Configuration of a Highly  Available 3-Tier Architecture on AWS For CRS Tech Ltd.
Overview
This project involved the design and manual deployment of a highly available 3-tier architecture on AWS, demonstrating expertise in cloud infrastructure and service integration. Using the AWS Management Console, I created a secure and scalable environment that included:
•	A custom VPC for isolated networking.
•	A bastion host for controlled access to private resources.
•	A web server for user-facing services.
•	An application server to manage business logic.
•	A RDS database for reliable data management.
The project highlights my ability to build robust cloud solutions, focusing on high availability, security, and performance.

Technologies Used
•	Amazon Virtual Private Cloud (VPC)
•	Amazon EC2
•	Amazon RDS


Network Topology

![image](https://github.com/user-attachments/assets/c8a545ba-119a-4962-b325-9af6f84f6bf5)

 
Deployment Steps
Step1 : Created a custom VPC on the AWS management console
What is a VPC: It stands for a Virtual Private Cloud, which is a logical isolated virtual network that you define to launch your AWS resources.
-	Go to the AWS console (login with your credentials)
-	Search VPC and click on the create VPC
-	Select VPC only and give it a name: CRSTech-VPC
-	IPV4 CIDR block used – 192.168.0.0/16
-	Leave everything else and click Create VPC

![image](https://github.com/user-attachments/assets/0f4e9748-d729-4581-afda-7bb1610b292c)


Step2: Created 4 subnets (1 public & 3 private subnets) in two Availability Zones (AZ)
What is a subnet: It is a range of IP addresses in your VPC, where AWS resources(e.g. EC2 instances) can be created.
The following are the created subnets with their corresponding IPV4 CIDR block addresses.
i.	public subnet1 – 192.168.1.0/24
ii.	private subnet1 – 192.168.2.0/24 
iii.	private subnet2 – 192.168.3.0/24 
iv.	private subnet3 – 192.168.4.0/24 

•	NB: Select the VPC you created when prompted.

•	Assign names, Availability Zones and IP addresses to the subnets and click create subnet when done.

![image](https://github.com/user-attachments/assets/ad299321-d755-45c9-a1b0-a7a5ba980ab6)

![image](https://github.com/user-attachments/assets/890265e0-e455-48ea-9aa2-1cb6663427fc)

![image](https://github.com/user-attachments/assets/89309f6c-19e8-44c2-b22d-f774b61052b0)

![image](https://github.com/user-attachments/assets/6f3677ce-2c24-4946-806c-57fb5cb9d54b)



•	Screenprint of the subnets created.
 
![image](https://github.com/user-attachments/assets/89a756a4-0e3c-4372-bae9-b549d8d33cf6)


Step3 – Allocated an Elastic IP
Elastic IP: It is a static IPV4 address designed for dynamic cloud computing.
•	Go to the left hand pane, select Elastic IPs and click Allocate Elastic IP 
•	Ensure that your region matches the network border group selected and click allocate when verified.

![image](https://github.com/user-attachments/assets/30d936f3-8f5a-4f7a-8c8d-0bebcdd069f8)

![image](https://github.com/user-attachments/assets/1e518451-b69d-47e2-b1b3-8673fe743cfe)

![image](https://github.com/user-attachments/assets/fa8ec6ff-1c15-49d7-809c-b65ac9817834)



Step4 : Created an Internet Gateway(IGW) and Attached It to the VPC
Internet Gateway: It allows the communication between your VPC and the internet. It is horizontally scaled, redundant and highly available.

•	On the left hand pane, Select Internet Gateway and click Create Internet Gateway.
•	Name tag = CRSTech-IGW

![image](https://github.com/user-attachments/assets/ffa3bb8d-b9af-4b79-8705-421d76060d09)


•	Attach the IGW to the VPC once its created
•	Select the VPC you created and click Attach internet gateway
         
![image](https://github.com/user-attachments/assets/04c04ad6-b3ef-45c6-8d1a-b7a2da3b4007)

 

Step5: Created a NAT gateway
Nat Gateway: It is a Network Address Translation (NAT) service which allows instances in a private subnet connect to services outside your VPC but external services cannot establish a connection with those instances.
•	Create a NAT Gateway by clicking on Nat Gateways on the left hand pane and then click “Create NAT Gateway”

![image](https://github.com/user-attachments/assets/0ab92ffd-f5d7-416b-95e5-315a67128c41)


•	Assign a name to the NAT Gateway
•	Subnet – Select the public subnet
•	Elastic IP allocation ID – Select the one you created
•	Click “Create NAT gateway”

![image](https://github.com/user-attachments/assets/9bb0bb84-a8a9-430b-a60c-073e88607886)



Step6: Created two Route Tables
Route Table: This contains a set of rules (routes) that determine where network traffic from your subnet or gateway is directed.


•	Create a Route Table by navigating to the left hand pane of the console and select “Route tables” and click “create route table”.

![image](https://github.com/user-attachments/assets/a06ff503-d48c-4238-a5cc-d9c7c6d47408)

•	Assign a name to the route table indicating that it is the public route table
•	Select your VPC when prompted 
•	Click Create route table

![image](https://github.com/user-attachments/assets/3524c57d-80d7-4e01-ad2b-9507ff0287cd)


•	Create a second route table indicating it is the private route table (same steps as before)

![image](https://github.com/user-attachments/assets/58b4ba60-47e3-4dfb-9d56-b7e86c224f9f)


 
•	Associate your subnets to the respective route tables.
i.	Public subnet1 – public-rtb
ii.	Private subnets(X3)  - private-rtb
•	Go to Route tables
i.	Select public-rtb 
ii.	subnet associations 
iii.	and select “Edit subnet associations”

![image](https://github.com/user-attachments/assets/598f8a48-b58c-4024-a1e1-c3da8c31e897)

 
•	Select the public subnet and click “Save Associations”

![image](https://github.com/user-attachments/assets/f3540b3f-8b0e-4a3d-ab8f-5333ee7333dc)

 

•	Add a route to the public route table to get access to the internet gateway.
•	Select Routes and Click on the Edit routes

![image](https://github.com/user-attachments/assets/23c1d4cd-8e7c-4437-8978-38e43c96d11b)
 

•	Click Add route
i.	Destination “0.0.0.0/0  (Anywhere)” 
ii.	Target should be your Internet gateway.
•	Save Changes

![image](https://github.com/user-attachments/assets/e3ec0e99-49a3-4289-a1c6-afd7d34987c8)


 
•	Associate the private subnets to the private route table (same steps as the public subnet association) but remember to select all 3 private subnets.

![image](https://github.com/user-attachments/assets/47f6321a-43fa-471a-a69b-dd07ca3e6740)

![image](https://github.com/user-attachments/assets/ebea6f2e-a896-428a-9e11-6a1915f37111)


•	Now Edit the Routes of the private route table to add the NAT gateway

![image](https://github.com/user-attachments/assets/44d7d86e-02c6-45b1-bac4-331700cc9f4d)

 
•	Click Add route
i.	Destination = 0.0.0.0/0 
ii.	Target = NAT gateway

![image](https://github.com/user-attachments/assets/3786f6e0-0e03-4420-8c97-6f9d80280e49)

![image](https://github.com/user-attachments/assets/63609ae4-fe76-4fa4-ba25-c91b57c28506)



Step7: Created 4 Security groups
Security group: This is a virtual firewall for EC2 instances to control inbound and outbound traffic.
The following security groups were created:
i.	Bastion Host security group
ii.	App Server security group
iii.	Web Server security group
iv.	Database Server Security group

•	Create security group by navigating to the left hand pane of the console, select security groups and click “Create security group”

![image](https://github.com/user-attachments/assets/2f7cba91-ae46-4c1e-a574-98971ac1c0b9)


•	Created the Bastion Host Security group
NB: Remember to always select the VPC you created when prompted.

![image](https://github.com/user-attachments/assets/76f4a2ea-4bc8-413e-a29c-e8c655ff8aa5)


•	Added 3 inbound rules by clicking Add rule
i.	Rule 1 – Opened port 80 for HTTP connection with source Anywhere- IPV4.
ii.	Rule 2 – Opened port 443 for HTTPS connection with source Anywhere-IPV4.
iii.	Rule 3 – Opened port 22 for SSH connection with source Anywhere-IPV4.

![image](https://github.com/user-attachments/assets/cae27bd4-158b-4dd9-9137-b5888f6a1713)


![image](https://github.com/user-attachments/assets/a1eb6840-6f0c-4d78-a3de-dbff3feeb491)


•	Created the Webserver security group
  Added the same inbound rules as the Bastion Host-SG

![image](https://github.com/user-attachments/assets/592d8b6d-7ee5-43f7-94da-29cacb7d1cee)


![image](https://github.com/user-attachments/assets/a305e2cd-695a-42b3-a8d1-8880b5f33f56)

 
•	Created the App Server Security group.
Added 2 inbound rules
i.	a. Type: All ICMP-IPV4
b. Source: WebServer-SG
ii.	a. Type: SSH 
b. Source: BastionHost-SG

![image](https://github.com/user-attachments/assets/9d24200e-f8a2-487e-bda3-68f565cf9c40)

![image](https://github.com/user-attachments/assets/a0e45b8b-6c96-4705-a601-1f7704c7938c)

 
•	Created the Database Server security group.
Added 2 inbound rules 
i.	a. Type: MYSQL/Aurora
b.	Source: App Server SG
ii.	a. Type: MYSQL/Aurora
b. Source: Bastion Host SG

![image](https://github.com/user-attachments/assets/0590e5dc-d0e7-4c73-b9c3-5e3ad41bd613)

![image](https://github.com/user-attachments/assets/df5b065d-d8c2-41b4-9f4c-028e0e6d2385)


•	Go back to Security groups in the left pane and add the following inbound rules:
i.	BastionHost-SG
a.	Type: MYSQL/Aurora
b.	Source: DatabaseServer-SG
ii.	WebServer-SG
a.	Type: All ICMP – IPV4
b.	Source: AppServer-SG
iii.	AppServer-SG
a.	Type - MYSQL/Aurora
b.	Source – DatabaseServer-SG
iv.	AppServer-SG
c.	Type - HTTP
d.	Source – 0.0.0.0/0


Step8: Server Deployment (EC2 instances)
EC2 instance is a virtual server in the AWS cloud that allows users to run applications on AWS.

•	Created the Bastion Host Server by navigating to Services and searched for EC2 instances.
i.	Selected instances on the left pane of the console and clicked “Launch instances” and configured the following.
     
![image](https://github.com/user-attachments/assets/fd4880b7-95b7-4ef6-b16c-4ee913622d72)
 

ii.	Name: BastionHostServer
iii.	AMI: Amazon Linux2 AMI (HVM)

![image](https://github.com/user-attachments/assets/1c1a4a59-4b06-441a-8911-afeb2ef7fd47)


iv.	Instance type: t2.micro

![image](https://github.com/user-attachments/assets/5ea6d27b-c980-4a85-ae6a-ac60f3589543)


v.	Key pair = select vockey and (download from the labpage)

![image](https://github.com/user-attachments/assets/85f399e6-55ac-4e6d-94fa-f4c42cc85d87)


Under Network Settings
vi.	Select your VPC
vii.	Select the public subnet
viii.	Enable Auto-assign public IP
ix.	Select an existing security group
x.	Select the BastionHost-SG
xi.	Leave everything else as it is and click Launch instance.

![image](https://github.com/user-attachments/assets/f55fbaea-0d06-441f-8ed0-94612088c82f)

 

•	Created the Web Server with the following configuration.
i.	Name: WebServer
ii.	AMI: Amazon Linux2 AMI (HVM)

![image](https://github.com/user-attachments/assets/6b4572e7-abc4-4979-a4ac-5a92f5817cfb)


iii.	Instance type: t2.micro
iv.	Key pair: vockey

![image](https://github.com/user-attachments/assets/da4e3ef9-f13c-43b6-bab2-1f3da63798d7)

 

Under Network Settings
v.	Select your VPC
vi.	Select the public subnet
vii.	Enable Auto-assign public IP
viii.	Select an existing security group
ix.	Select the WebServer-SG

![image](https://github.com/user-attachments/assets/c7eccda4-9dbe-48bf-9932-bf0f003b392d)

 

x.	Added the script below in the user data section located at the Advance Details section at the bottom page.

#!/bin/bash
sudo yum update -y
sudo amazon-linux-extras install -y lamp-mariadb10.2-php7.2 php7.2
Sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd


![image](https://github.com/user-attachments/assets/8db3e4b2-d83a-44e3-8385-2be952fc3605)


xi.	Click Launch instance after inputting the script.

•	Created the App Server with the following configuration details:
i.	Name: AppServer
ii.	AMI: Amazon Linux2 AMI (HVM)

![image](https://github.com/user-attachments/assets/15e66621-fff8-41a9-8c4a-c4e5060bf23b)


iii.	Instance type: t2.micro

![image](https://github.com/user-attachments/assets/402da4c7-4933-462b-af5c-18581e982059)


Under Network Settings
iv.	Select your VPC
v.	Choose private subnet1
vi.	Disable Auto-assign public IP
vii.	Select an existing security group
viii.	Choose the AppServerSG

![image](https://github.com/user-attachments/assets/9b84bf89-aa8e-408b-abb5-048769d17348)
 

ix.	Added the script below in the user data section located at the Advance Details section at the bottom page

#!/bin/bash
sudo yum install -y mariadb-server
sudo service mariadb start

![image](https://github.com/user-attachments/assets/60aa7907-a29f-4a02-8809-a4451956f6e0)
 

x.	Launch instance


Step9 : Database Creation
i.	Search Amazon RDS in services and on the left pane of the console select subnet groups
ii.	Select Create DB subnet group

![image](https://github.com/user-attachments/assets/3dd980e4-3e7a-4017-8f39-6b13d92a9407)
 

Subnet Group Configuration
iii.	Name: Assign a name (DBSubnetGroup) 
iv.	Description:
v.	Select your VPC
vi.	Select the AZ where you deployed your subnets
vii.	Select private subnets 2 & 3
viii.	Click Create

![image](https://github.com/user-attachments/assets/38018209-b7e3-4697-9a85-ef9094c95f70)


Screen print of the subnet group created.

![image](https://github.com/user-attachments/assets/5ee9f0e8-b661-4e39-a47a-41c1a514bed5)


•	Go to the left pane and select Databases
•	Click create database with the following configuration

![image](https://github.com/user-attachments/assets/fb65bdb9-80db-4762-aa12-66290bfbe8d8)

-	Creation Method: Standard create
-	Engine type: MariaDB
-	Engine version: MariaDB 10.11.9

![image](https://github.com/user-attachments/assets/867f1523-439f-41ad-a614-164a19b2e3b4)


![image](https://github.com/user-attachments/assets/75531b25-d8af-47eb-9f5c-b66f869f108c)

-	Template

![image](https://github.com/user-attachments/assets/af1bc3d7-6ce7-4d2e-8922-b2434d2c8a76)

 
-	DB instance identifier: dbinstance
-	Credentials: Select self-manage and assign your own password.

 
![image](https://github.com/user-attachments/assets/78d44a4f-76de-49ff-8525-e9b6326aba6b)


•	Configure the following under Connectivity
i.	Assign your VPC
ii.	Make sure your subnet group is listed under the subnet group section
iii.	Public access: no
iv.	Choose existing VPC security groups
v.	Remove the default security group and add your database security group
vi.	Select the first availability zone

![image](https://github.com/user-attachments/assets/5af82448-3bce-4b8f-b4f1-1516f158e1c2)

![image](https://github.com/user-attachments/assets/c0f7012b-5b10-44c0-abc2-69037e09685b)


•	Configure the following under “Additional Configuration”
i.	Initial database name: mydb
ii.	Leave everything else as it is.
iii.	Click create database

![image](https://github.com/user-attachments/assets/f55d8588-c45e-4c5a-b3e9-15ac3ede928b)
 

•	Outcome

![image](https://github.com/user-attachments/assets/6e97c04b-82eb-42c9-a7a5-64aa3ddd078c)

![image](https://github.com/user-attachments/assets/febf29ed-661d-44af-8736-d21b9f8f902c)



•	Step10: Testing Connections (Windows machine)
i.	SHH into the BastionHostServer using Putty with the pem and ppk vockey files downloaded earlier.

![image](https://github.com/user-attachments/assets/4fdf2f1a-a76b-40fa-bcd2-7222b0e1de90)
 


ii.	Go into your powershell and type this command out (Pscp -scp -P 22 -i ’.\Downloads\labsuser.ppk’ -2 user ec2-user ‘.\Downloads\labsuser.pem’ ec2-user@35.94.52.131:/home/ec2-user)
iii.	NB: Your Bastion public IP address might be different.
iv.	Hit enter and it should upload those keys to your bastion host for use on other servers

![image](https://github.com/user-attachments/assets/64682ddf-fc9b-4e8b-81bc-c63515f7c8f6)


v.	Test your ssh to see if the file is uploaded by using the command “ls”. Should return something like this

![image](https://github.com/user-attachments/assets/21f80ce3-a2fc-43cd-bbeb-98b327b9c8c0)


vi.	Changed the file permission for the file downloaded to the bastion host by typing the following command: “chmod 400 labsuser.pem”
vii.	SSH into the AppServer by typing the following command:
ssh -i labsuser.pem ec2-user@192.168.2.187
viii.	Used “ls” command to verify that I was in a different server
 
![image](https://github.com/user-attachments/assets/54deec0c-3251-44cc-9c12-6c87d9088875)


•	Pinging from the webserver
i.	Used the ping command and the private IP address of the webserver to verify if there was a connection.
ii.	Had a successful ping.

![image](https://github.com/user-attachments/assets/d1dd4ec4-db41-4ac6-bc76-c8fe652dffd6)

 
•	Test out the database connection with the following command:
i.	mysql –user=admin -password=’_Mike_5177_’ –host=dbinstance.cbwmwmm7li1e.us-west-2.rds.amazonaws.com

![image](https://github.com/user-attachments/assets/126c8dd9-b7ff-4783-9c97-778f23f6f957)

![image](https://github.com/user-attachments/assets/b000c86e-1e38-4e16-942e-bd5d2319ae2d)



•	This concludes the project.


**Reflection**
Completing this project was a significant milestone in my cloud journey. It provided me with invaluable hands-on experience in designing and deploying a highly available 3-tier architecture using the AWS Management Console.
Through this process, I deepened my understanding of AWS services, including networking, compute, and database solutions, while emphasizing security and scalability best practices. The manual setup allowed me to develop a meticulous approach to configuration and troubleshooting, further enhancing my problem-solving skills.
This experience has not only strengthened my confidence in implementing cloud solutions but also reinforced my passion for creating innovative and efficient infrastructures. I'm excited to continue leveraging these skills to design impactful cloud architectures in future projects.











 



 







