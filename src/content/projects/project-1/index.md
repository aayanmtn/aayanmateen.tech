---
title: "SYSOPS AWS"
description: "Deploying Java Multi Tier on AWS"
date: "Mar 18 2024"
demoURL: ""
repoURL: "https://github.com/aayanmtn/java-multi-tier.git"
---


# **Deploying Multi-TierJAVA Application on AWS | SYOPS**



AWS Cloud | Nginx EC2 Apache Tomcat RabbitMQ MySQL

![https://miro.medium.com/v2/resize:fit:300/1*c7Xl8Rs605lNHq-tD-v2Cg.jpeg](https://miro.medium.com/v2/resize:fit:300/1*c7Xl8Rs605lNHq-tD-v2Cg.jpeg)

[https://github.com/aayanmtn/java-multi-tier.git](https://github.com/aayanmtn/java-multi-tier.git)

![https://miro.medium.com/v2/resize:fit:875/1*4gJ6Y8prUGz8zJEiIS1M5g.png](https://miro.medium.com/v2/resize:fit:875/1*4gJ6Y8prUGz8zJEiIS1M5g.png)

# **Security Groups**

We need to create security groups for instances.

![https://miro.medium.com/v2/resize:fit:713/1*4vrM6i_DyHwGT7G1JB0zLQ.png](https://miro.medium.com/v2/resize:fit:713/1*4vrM6i_DyHwGT7G1JB0zLQ.png)

by clicking on Create security group

inbound rules for elb0e-sg (load balancer)

![https://miro.medium.com/v2/resize:fit:875/1*XKG__XteUTUaCAnpl5SLSg.png](https://miro.medium.com/v2/resize:fit:875/1*XKG__XteUTUaCAnpl5SLSg.png)

inbound rules for app01-sg (for tomcat)

![https://miro.medium.com/v2/resize:fit:875/1*OuAfWBjbpb1BvPdd6hNjgQ.png](https://miro.medium.com/v2/resize:fit:875/1*OuAfWBjbpb1BvPdd6hNjgQ.png)

inbound rules for backend-sg (for tomcat)

![https://miro.medium.com/v2/resize:fit:875/1*S3FB6IC5G0tn5r23AucT-Q.png](https://miro.medium.com/v2/resize:fit:875/1*S3FB6IC5G0tn5r23AucT-Q.png)

consist rules for rabbitmq,memcache,MySQL

# **Key Pair**

Now we will create a new key pair by clicking on Create key pair

![https://miro.medium.com/v2/resize:fit:875/1*dZuGXuJa97sd7rqbv79N1Q.png](https://miro.medium.com/v2/resize:fit:875/1*dZuGXuJa97sd7rqbv79N1Q.png)

![https://miro.medium.com/v2/resize:fit:875/1*xi1UwB3mTVYsofDdpi5S3A.png](https://miro.medium.com/v2/resize:fit:875/1*xi1UwB3mTVYsofDdpi5S3A.png)

Insert the name.Use pem file if you are not using putty.This is very important to ssh aws instance from our computer.

# **Launch New EC2 Instances:**

![https://miro.medium.com/v2/resize:fit:300/1*c8bW38EQlLlJQUO11KON2w.jpeg](https://miro.medium.com/v2/resize:fit:300/1*c8bW38EQlLlJQUO11KON2w.jpeg)

![https://miro.medium.com/v2/resize:fit:875/1*P2G_F_JolzKByphEX2pK4g.png](https://miro.medium.com/v2/resize:fit:875/1*P2G_F_JolzKByphEX2pK4g.png)

- Navigate to the EC2 Dashboard and click on “Launch instance”.
- Use the following AMIs for the instances:
- RabbitMQ (`rbmq-01`): AlmaLinux 9 AMI
- Memcache (`mc-01`): AlmaLinux 9 AMI
- MySQL (`db-01`): AlmaLinux 9 AMI
- Tomcat Application (`app-01`): Ubuntu AMI
- Assign the respective security groups created earlier to each instance.
1. User Data Script:
- Paste the bash script located in the `userdata` directory of your repository into the "User data" section of the instance launch wizard.
- Launch the instances.

# **Building the Artifacts**

![https://miro.medium.com/v2/resize:fit:250/1*BLMRxVhEhC6fLPIioQulcw.png](https://miro.medium.com/v2/resize:fit:250/1*BLMRxVhEhC6fLPIioQulcw.png)

To build the artifact it is important to have JDK and Maven installed in your PC

![https://miro.medium.com/v2/resize:fit:875/1*NwX0u-tGYjea_gnMrjID2A.png](https://miro.medium.com/v2/resize:fit:875/1*NwX0u-tGYjea_gnMrjID2A.png)

1. Setup Development Environment:
- Ensure JDK and Maven are installed on your local machine.
- Configure Application Properties:
- Edit the `application.properties` file, changing `digitalsierra.in` to `<yourdomainname.xyz>`.
1. Build the Artifact:
- Run the following command to build the artifact:
- `mvn install`
1. Upload Artifact to S3:
- Install the AWS CLI on your local machine.
- Create a new IAM user in the AWS Management Console and obtain the access key and secret key.
- Configure the AWS CLI with the obtained credentials:
- Copy code
- `aws configure`
- Create a new S3 bucket:
- sh
- Copy code
- `aws s3 mb s3://<bucket-name>`

Copy the build artifact to the S3 bucket:

- `aws s3 cp target/vprofile-v2.war s3://<bucket-name>`

![https://miro.medium.com/v2/resize:fit:875/1*EeJRQ4l3uWhO4T4kZwG-lw.png](https://miro.medium.com/v2/resize:fit:875/1*EeJRQ4l3uWhO4T4kZwG-lw.png)

Create an user and get access and secret key

![https://miro.medium.com/v2/resize:fit:875/1*-qTSLN9PfdfYtaNMLpSc3g.png](https://miro.medium.com/v2/resize:fit:875/1*-qTSLN9PfdfYtaNMLpSc3g.png)

Use **aws configure** to configure cli

Now from your computer create an bucket **using aws s3 mb s3://<bucket name>**

then copy the vprofile-v2.war to the bucket use **cp /target/vprofile-v2.war aws s3 mb s3://<bucket name>**

![https://miro.medium.com/v2/resize:fit:70/1*BUN46e-sD9oJdXtM0DGy2w.png](https://miro.medium.com/v2/resize:fit:70/1*BUN46e-sD9oJdXtM0DGy2w.png)

Now access the App-01 ubuntu instance

and remove from tomcat9/webapps using this **rm-rf tomcat9/webapps/ROOT.war**

Copy aws s3://<bucket>/vprofile-v2.war to tomcat9/webapps/ROOT.war

![https://miro.medium.com/v2/resize:fit:875/1*AMNBSvsdNbWPwiq2T8ZaTA.png](https://miro.medium.com/v2/resize:fit:875/1*AMNBSvsdNbWPwiq2T8ZaTA.png)

Now lets Hop On to

# **Load Balancer and DNS**

![https://miro.medium.com/v2/resize:fit:875/1*6Nuo7N9KPDVkAmgG3Qmy-w.png](https://miro.medium.com/v2/resize:fit:875/1*6Nuo7N9KPDVkAmgG3Qmy-w.png)

Create a new target group it falls under load balancing in EC2

![https://miro.medium.com/v2/resize:fit:875/1*C1tjoyhmCJTJo6sWcCP47w.png](https://miro.medium.com/v2/resize:fit:875/1*C1tjoyhmCJTJo6sWcCP47w.png)

Use round robin

![https://miro.medium.com/v2/resize:fit:875/1*9_XFqOlvQ5KpegCbE4AnBw.png](https://miro.medium.com/v2/resize:fit:875/1*9_XFqOlvQ5KpegCbE4AnBw.png)

Unified configuration and create.

![https://miro.medium.com/v2/resize:fit:348/1*cvM4k7GZP2fTHqkl_g58Cg.png](https://miro.medium.com/v2/resize:fit:348/1*cvM4k7GZP2fTHqkl_g58Cg.png)

![https://miro.medium.com/v2/resize:fit:875/1*lJxqlWMH-B_Q3g7B0gLf2w.png](https://miro.medium.com/v2/resize:fit:875/1*lJxqlWMH-B_Q3g7B0gLf2w.png)

Use Application Load balancer

check all mappings use HTTP port 80 And use the above target group

![https://miro.medium.com/v2/resize:fit:875/1*H5pb_nNw7kaXOJbxO4L8bQ.png](https://miro.medium.com/v2/resize:fit:875/1*H5pb_nNw7kaXOJbxO4L8bQ.png)

If you have you have acm certificate you can add https port 443

![https://miro.medium.com/v2/resize:fit:875/1*2WCRhL5BZP9PzIdqfW7GOA.png](https://miro.medium.com/v2/resize:fit:875/1*2WCRhL5BZP9PzIdqfW7GOA.png)

Use DNS Name to access application

![https://miro.medium.com/v2/resize:fit:875/1*StnsB2IqXJcxAUxdoz7C7g.png](https://miro.medium.com/v2/resize:fit:875/1*StnsB2IqXJcxAUxdoz7C7g.png)

You can see login using admin_vp:admin_vp

![https://miro.medium.com/v2/resize:fit:875/1*8nsXyOaNlbqytzft6gooTA.png](https://miro.medium.com/v2/resize:fit:875/1*8nsXyOaNlbqytzft6gooTA.png)

[AWS](https://medium.com/tag/aws?source=post_page-----7a8ffb3cdf20---------------aws-----------------)

[Aws Elb](https://medium.com/tag/aws-elb?source=post_page-----7a8ffb3cdf20---------------aws_elb-----------------)

[S3 Bucket](https://medium.com/tag/s3-bucket?source=post_page-----7a8ffb3cdf20---------------s3_bucket-----------------)

[Multi Tier](https://medium.com/tag/multi-tier?source=post_page-----7a8ffb3cdf20---------------multi_tier-----------------)

[Tomcat](https://medium.com/tag/tomcat?source=post_page-----7a8ffb3cdf20---------------tomcat-----------------)
