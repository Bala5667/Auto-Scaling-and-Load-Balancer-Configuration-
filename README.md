# Auto-Scaling-and-Load-Balancer-Configuration-

Main Objecetives:
1. Create a Launch Template
2. Configure Auto Scaling Group
3. Set Up an Elastic Load Balancer (ELB)
4. Traffic Distribution and High Availability
5. Testing and Verification

Steps to achive the Objectives:


1. Created a Launch Template
I set up a Launch Template using an existing EC2 instance and added a script to install Apache and create an HTML file.

Launch Template User Data Script :
#!/bin/bash
sudo yum update -y
sudo yum install -y httpd
sudo systemctl start httpd
sudo systemctl enable httpd
echo "Hello, this is an Auto-Scaling EC2 Instance - $(hostname -f)" | sudo tee /var/www/html/index.html
sudo systemctl restart httpd

Steps:

AWS Console → EC2 → Launch Templates → Create Launch Template.
Choose an existing EC2 instance or configure a new one.
Added User Data (above script) to configure Apache automatically.
Selected the AMI, instance type, key pair, and security group.
Created the Launch Template.
   
2. Created Auto Scaling Group
After setting up the Launch Template, I configured Auto Scaling to automatically create and manage instances.

Steps:
AWS Console → Auto Scaling Groups → Create Auto Scaling Group.
Selected the Launch Template created earlier.
Configured:
o	Minimum instances: 1
o	Desired instances: 2
o	Maximum instances: 3
Enabled Health Check and Monitoring.
Selected Attach to an existing Load Balancer and chose a Target Group (created in the next step).
Clicked Create Auto Scaling Group.

3. Created a Load Balancer and Target Group
Since I wanted to distribute traffic across instances, I added a Load Balancer and Target Group.

Steps:
AWS Console → Load Balancers → Create Load Balancer.
Selected Application Load Balancer (ALB).
Configured:
o	Scheme: Internet-facing
o	Listeners: HTTP (Port 80)
o	VPC & Subnets: Selected the appropriate network.

5.	Created a Target Group:
   
o	AWS Console → EC2 → Target Groups → Create Target Group.
o	Target Type: Instance
o	Protocol: HTTP (Port 80)
o	Selected the Auto Scaling instances.
Registered EC2 instances to the Target Group.
Attached the Target Group to the Load Balancer.
Saved configurations and created the Load Balancer.

4. Tested Auto Scaling & Load Balancer
Once everything was configured, I verified the setup.

Check Load Balancer DNS Name
curl http://<Load_Balancer_DNS>
•	Successfully loaded the index.html page from one of the EC2 instances.
•	With each refresh, the response is served from a different IP address associated with the instances in the Auto Scaling Group. This occurs because the Load Balancer distributes incoming traffic across multiple instances, ensuring load balancing and high availability.Deleted an EC2 Instance to Test Auto Scaling

Went to EC2 Instances and manually terminated one instance.
Auto Scaling automatically launched a new instance within a few minutes.

Checked the Load Balancer again:
curl http://<Load_Balancer_DNS>
The website was still accessible, proving Auto Scaling is working.

Project Outcome :

Auto Scaling successfully created and managed EC2 instances automatically.
Load Balancer distributed traffic evenly across instances.
Deleting an instance triggered Auto Scaling to launch a new instance.
Website remained accessible without downtime.
This project demonstrated how Auto Scaling and Load Balancers improve reliability and scalability in AWS. 

