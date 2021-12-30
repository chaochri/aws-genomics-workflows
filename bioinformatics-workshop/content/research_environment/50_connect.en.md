---
title: "Connect to the instance"
weight: 50
---

Navigate to the EC2 console (you can search for it in the search bar) and click 
on instances on the left side menu.

![ec2 menu](/static/images/compute_and_storage/13_ec2_menu.png)

On this page you can find the instance you’ve just created. Check the box next 
to the instance to see details in the lower pane. Note the public IPv4 address, 
we are going to use this to connect to the instance.

![ec2 instances](/static/images/compute_and_storage/14_ec2_instances.png)

If you are using Linux or Mac, open a terminal window and run the following 
command.

:::code{showCopyAction=true showLineNumbers=true language=bash}
ssh -i ~/.ssh/biotech-research.pem ec2-user@IP-ADDRESS
:::

Change biotech-research.pem to the name you gave to your ssh key pair. 
Replace IP-ADDRESS with the IP address you noted from the EC2 instance.

::alert[ec2-user is the default user on an Amazon Linux. If you use a different distribution this user might be different.]

If you are using Windows, please refer to the 
[following instructions](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/putty.html).

When connected successfully you should see something like the following.

![ssh first time](/static/images/compute_and_storage/15_ssh_first_time.png)