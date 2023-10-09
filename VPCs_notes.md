## VPCs - Virtual private clouds 

Analogy
1. Building --> AWS (many cloud computing platforms)
2. Shared Flats --> Default VPC
3. Shared Rooms --> Default subnets (1a, 1b, 1c)


Custom VPCs:
- have more control over security setup
- More control
- Own security setup

#### How to setup custom VPC
1. 
2. Go to VPC in AWS Search bar
NOTE: Make sure the the CIDR block do not overlap if they need to talk to each other
2. Create VPC
3. Select VPC Only
4. Name tag = `tech254-wafa-2tier-first-vpc`
5. Select IPv4 CIDR manual input
5. IPv4 CIDR = `10.0.0.0/16`
6. Select No IPv6 CIDR block
7. Name tag is already done for you 
8. Click Create VPC
NOTE: Should say State available very quickly
9. On left navigation click Subnets
10. Create Subnets
11. Search for your name VPC 
12. Subnet settings > subnet 1 of 1 = `public-subnet`
13. Choose availability zone = `Europe (ireland) / eu-west-1 1a`
14. IPv4 subnet CIDR block = `10.0.2.0/24`
NOTE: should tell you number of IP address in that space in right hand
15. Name tag already done for you 
16. Add New Subnet
17. Subnet name = `private-subnet`
18. Choose availability zone = `Europe (ireland) / eu-west-1 1b`
19. IPv4 subnet CIDR block = `10.0.3.0/24`
20. click `Create Subnets`

Create Internet Gateway
1. Select Internet Gateway in navigation plane
2. Create Internet Gateway 
3. Name tag = `tech254-wafa-2tier-first-vpc-ig`
4. `Create internet gateway `
5. Click **Actions** > `Attach a VPC`
6. Search for your VPC and select
7. Select `Attach to Internet Gateway`

Create public Route Table
1. Got go to Route Tables
2. Create Route Tables
3. Name tag = 'public-rt'
4. Create Route Table
Create Association (connection)
5. Scroll down to middle of the page > **Subnet Associations**
6. Click `Edit Subnet Association`
7. Check the `puclic-subnet` option

Internet Gateway part of Route (Add IG to Route)
1. On Routes page, scroll down and select **Routes** tab
2. Click `Edit Routes` 
3. Add Route
4. Destination = `0.0.0.0/0`
5. Under Target > Select Internet Gateway in dropdown > Select your name gateway (doube dropdown) 

To check its done correctly:
1. Go to Your VPCS
2. Click in VPC ID
3. Scroll down to Map
4. Public RT should be set to the public cocrrectly in the tree diagram 

Add VM for app and Database
DATABASE
1. EC2
2. AMI's
3. Search Ramons name
5. Use AMI with `tech241-ramon-user-data-ami`
NOTE: YOU WILL NEED TO USE YOUR OWN AMI IN THE FUTURE. Or create it
6. Check the box
3. Launch Instance from AMI
4. Name = `tech254-wafa-db-test-first-vpc`
5. Key pair = tech254
6. Network settings > Edit settings
7. VPC > select your name (we made this)
8. Subnet > private-subnet
9. Disable
10. Create security Group
11. Custom TPC > Port 27017 > Aywhere

APP
1. Instances
2. AMIS
3. Search your name (this case ami3)
4. check the box 
5. Launch Instance from AMI
4. Name = `tech254-wafa-app-test-first-vpc`
5. Key pair = tech254
6. Network settings > Edit settings 
7. VPC > select your name (we made this)
8. Subnet > public-subnet
9. Enable
10. Create security Group
11. Custom TPC > Port 3000 > Aywhere
12. Add HTTP too > Anywhere
13. Expand Advanced details 
14. Copy and paste Under User Data:
```commandline
#!/bin/bash

export DB_HOST=mongodb://10.0.3.165:27017/posts

#install the app 
cd /home/ubuntu/repo/app

#sudo systemctl restart nginx
npm install

node seeds/seed.js

sudo npm install pm2 -g
pm2 kill
pm2 start app.js
```
NOTE: replace the IP with your **private** IP address in your DB instance
15. **Create Instance**

Check its worked PROPERLY
1. Click on `tech254-wafa-app-test-first-vpc` instance created
2. Copy and Paste IP Address into a browser
3. Sparta App should be displayed
4. Add `/posts` at the end of the IP (IP Address/posts) 
5. Sparta posts page should be displayed 

Cleaning up resources/deleting
1. Terminate Instances (DB and app) first
2. Delete Security Groups labelled first-vpc
3. VPC > Delete VPC. It will delete VPC and the following too:
   4. Route Table
   5. Internet gateway
   6. and Subnets (private and public) 


