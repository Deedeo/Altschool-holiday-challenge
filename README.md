# ALTSCHOOL HOLIDAY CHALLENGE

 **Creating a VPC with Private EC2 Instances and a  Application Load Balancer using the AWS Management Console.**
 
### IMPORTANT INSTRUCTIONS
* Set up 2 EC2 instances on AWS(use the free tier instances).
* Deploy an Nginx web server on these instances(you are free to use Ansible)
* Set up an ALB(Application Load balancer) to route requests to your EC2       instances
* Make sure that each server displays its own Hostname or IP address. You      can use any programming language of your choice to display this.
* Define a logical network on the cloud for your servers.
* Your EC2 instances must be launched in a private network.
* Access must be only via the load balancer
* You must submit a custom domain name(from a domain provider e.g. Route53)   or the ALB’s domain name.

##  GETTING STARTED

### PREQUITISTES
* An AWS account
  - Access to the AWS Management Console with the necessary permisions.
  

### GOAL
* ````This illustrates how an Application Load Balancer attached to EC2 instance running in a private subnet can be configured to be accessed from the internet the help of a aws service called Nat gateway.````

### Architecture Diagram
  ![Architecture Diagram](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/alt.drawio.png)



#### Overview
- In order to achieve the goal of this holiday challenge, the following steps are required.

#### STEP 1: CREATE VPC
   > On the VPC configuration Dashboard choosing VPC and more automatically launches Private Subnets, Public Subnets, Route Tables, Internet GateWay, Elastic IP, Availability Zones within your current region and the necessary configuration are done.

    * create a new VPC with the IPv4 CIDR block 192.168.0.0/16.
    * The VPC is configured with in US-east-1 region.
    * The VPC has 2 private Subnets and 2 public Subnets.
    * The VPC has 2 private route table and a public route table.
    * Internet Gateway.
    * Nat Gateway.

![VPC](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/nayaG.jpg)

****please note that Elastic-IP will be allocated to Nat-gateway     automatimacally****

 

#### STEP 2 CREATE A SECURITY GROUP FOR PRIVATE INSTANCES AND APPLICATION LOAD BALANCER
##### step 2a:  create Load balancer security group in the new VPC network
        - Allow all traffic on port 80 from 0.0.0.0/0. 
        - Allow all traffic on port 443 from 0.0.0.0/0.

![SecurityGroup1](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/lg-sg.jpg)
##### step 2b: create private ec2 security group in the new VPC network
        - Allow SSH on port 22 from my IP address.
        - Allow All Traffic from load balancer security group.

![SecurityGroup2](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/images/naya-sg.jpg)
  

#### STEP 3: LAUNCH TWO PRIVATE EC2 INSTANCES WITH A USERDATA
         - Create ec2 instance using amazon linux 2.
         - Create a key pair or use the existing key pair.
         - Select the new VPC under the network setting
         - Select a private subnet in any AZ created at step 1
         - Select the security group created for private instances.
         - Pre-configure instances with a bash script that install nginx and echo an index file into the Nginx config.
         - Launch the instances

![EC2](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/network-naya.jpg)

![EC2](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/server-naya2.jpg)

![EC2](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/naya-userdata.jpg)

#### STEP 4: CREATE A TARGET GROUP
         - Give Target group a name.
         - Select instances as the target.
         - Select the private instances and register them with target group.
         - click Create target group.
![TG](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/tg-naya.jpg)



#### STEP 5 CREATE AN APPLICATION LOAD BALANCER
         - Give ALB a name;
         - Schema: internet-facing;
         - Ip-adrress: iPV4;
         - Network-mapping: The new VPC;
         - Select at least two public subnets;
         - Attach load balancer security group;
         - Set listener to 80;
         - Attach target group.
         - create Application Load Balancer.

![ALB](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/lb-naya.jpg)

![ALB](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/healthy-naya.jpg)

> Using the Application Load Balancer DNS you can confirm the status of your server.

![ALB-DNS](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/hello1.jpg)
 > Then after a refresh the load balancer send traffic to the second server 

![ALB-DNS](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/hello2.jpg)

resources ![aws docs](https://docs.aws.amazon.com/)
#### STEP 6: CREATE A HOSTED ZONE ON ROUTE 53
     - create hosted zone
     - create a record to map the application load balancer to your domian name.
![domain1](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/dns1.jpg)

![domain2](https://github.com/Deedeo/Altschool-holiday-challenge/blob/master/images/dns2.jpg)