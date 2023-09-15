# Chapter 10. Elastic Load Balancing (ELB)

<!-- TOC -->

- [Chapter 10. Elastic Load Balancing ELB](#chapter-10-elastic-load-balancing-elb)
  - [Lab 10. Use Application Load Balancers for Web Servers](#lab-10-use-application-load-balancers-for-web-servers)
    - [Introduction](#introduction)
    - [Runbooks](#runbooks)
      - [Create a Second Server](#create-a-second-server)
      - [Create an Application Load Balancer ALB](#create-an-application-load-balancer-alb)
      - [Enable Sticky Sessions](#enable-sticky-sessions)

<!-- /TOC -->

---
## Lab 10. Use Application Load Balancers for Web Servers

### Introduction

You will configure an Application Load Balance (ALB) to distribute network traffic to two EC2 instances. You will then enable stickiness, so that once a server is contacted, the user is always sent to that server.

This ensures our legacy application continues to work despite not supporting distributed logins.

### Runbooks

1. Create a Second Server.

2. Create an Application Load Balancer (ALB).

3. Enable Sticky Sessions.

<details>
<summary>Click here to start Lab 10.</summary>

#### 1. Create a Second Server

1. Navigate to the AWS console > EC2 > click **Launch instances**.

2. Enter the following values:
  - Under **Name and Tags**, enter `webserver-02`.
  - Under **Application and OS Images (Amazon Machine Image)**, select Ubuntu > Ubuntu Server 22.04 LTS.
  - Under **Instance Type**, select `t3.micro`.
  - Under **Key pair (login)**, select Proceed without a key pair.
  - Under **Network settings**, click Edit > set Auto-assign Public IP to Enable.
  - Under **Firewall (security groups)**, click Select existing security group > select `EC2SecurityGroup`.

3. Under **Advanced Details**, in the User Data box, enter the following bootstrap script:

```sh
#!/bin/bash
sudo apt-get update -y
sudo apt-get install apache2 unzip -y
echo '<html><center><body bgcolor="black" text="#39ff14" style="font-family: Arial"><h1>Load Balancer Demo</h1><h3>Availability Zone: ' > /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/placement/availability-zone >> /var/www/html/index.html
echo '</h3> <h3>Instance Id: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/instance-id >> /var/www/html/index.html
echo '</h3> <h3>Public IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/public-ipv4 >> /var/www/html/index.html
echo '</h3> <h3>Local IP: ' >> /var/www/html/index.html
curl http://169.254.169.254/latest/meta-data/local-ipv4 >> /var/www/html/index.html
echo '</h3></html> ' >> /var/www/html/index.html
sudo systemctl start apache2
sudo systemctl enable apache2
```

4. Click **Launce Instance** > click the Instance ID, and once it's in the Running state > copy the Public IPv4 address.

5. In a new browser tab, paste the IP address you copied.
  - You should see the load balancer demo page.

#### 2. Create an Application Load Balancer (ALB)

1. Back in the EC2 > click Load Balancers > click **Create Load Balancer**.

2. From the Application Load Balancer card, click Create.
  - For **Load balancer name**, enter `LegacyALB`.
  - Under **Network mapping**, click the VPC dropdown and select the provided VPC.
  - Under **Availability Zones**, select each of `us-east-1a`, `us-east-1b`, `us-east-1c`.
  - Under **Security groups**, deselect the default security group, and from the dropdown select `EC2SecurityGroup`.
  - Under **Listeners and routing**, set the Protocol to HTTP > set Port to 80.

3. Under **Default action**, click Create target group, which will open in a new tab.
  - For **Target group name**, enter `TargetGroup` > click Next.
  - Under **Available instances**, select both targets that are listed > click Include as pending below.
  - Click **Create target group**.

4. Back in the first tab:
  - Under **Default action**, click the refresh button > select the `TargetGroup` you just created.
  - Click **Create load balancer**.

5. On the next screen, click View load balancer.
  - After the load balancer is in an active state, copy its **DNS name**.

6. In a new browser tab, paste the copied DNS name.
  - You should see the load balancer demo page again.
  - The local IP lets you know which instance you were sent to.

#### 3. Enable Sticky Sessions

1. Back to the Load Balancer tab > select the load balancer > click **Listeners and rules**.

2. Click the **TargetGroup** link in the Default action column > select Attributes > click Edit.

3. Check the box next to Stickiness to enable it > set Stickiness type to Load balancer generated cookie > set Stickiness duration to 1 days > click **Save changes**.

4. Back to the browser tab with the load balancer's IP > refresh the browser page.
  - This time, no matter how many times you refresh, it will stay on the same instance (noted by the local IP).
