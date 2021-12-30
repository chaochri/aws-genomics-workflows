---
title: "Cloud9"
weight: 70
---

An alternative way to setup EC2 with all the required permissions is to use 
[AWS Cloud9](https://aws.amazon.com/cloud9/).
Cloud9 is a cloud-based integrated development environment (IDE) that allows 
you to write, run, and debug your code with just a browser. It includes a code 
editor, debugger, and terminal.  
Instead of creating an EC2 instance, managing it, and connecting via SSH, we 
can launch a fully managed IDE (with terminal support) in the cloud and connect 
to it through the AWS web console. 


## Create a Cloud9 environment

Search for "Cloud9" in the AWS web console search bar and click on "Cloud9".

![Search Cloud9](/static/images/compute_and_storage/19_find_cloud9.png)

Click on "Create environment".

![Cloud9 create button](/static/images/compute_and_storage/20_cloud9_create_environment_button.png)

Name the environment (e.g., biotech-research) and add an optional description.

Click on "Next step".

![Cloud9 create 1](/static/images/compute_and_storage/21_cloud9_create_environment1.png)

**Environment type** \- Choose "Create a new EC2 instance for environment 
(direct access). This will create a new EC2 instance managed by the Cloud9 
service.  
**Instance type** \- Select "Other instance type" and choose c5.4xlarge.  
**Platform** \- Choose "Ubuntu Server 18.04 LTS". This time we are going to 
show that we can use a different OS than Amazon Linux 2.  
**Cost-saving settings** \- keep the default setting to "After 30 minutes 
(default)". This will stop the instance if there’s inactivity for 30 minutes. 
You can change this if you are planning to let the instance run for a long time 
with no interaction (i.e., run long processes while not using the IDE). 

![Cloud9 create 2](/static/images/compute_and_storage/22_cloud9_create_environment2.png)

**Tags** \- Add the same tags we've used before, i.e., Environment=Dev and 
Project=Biotech.

![Cloud9 create 3](/static/images/compute_and_storage/23_cloud9_create_environment3.png)

Click on "Next step".

Review the settings and then click on "Create environment".

![Cloud9 create 4](/static/images/compute_and_storage/24_cloud9_create_environment4.png)

![Cloud9 create 5](/static/images/compute_and_storage/25_cloud9_create_environment5.png)

Cloud9 will start a new instance, and you should get an indication while it's 
creating the new environment.

![Cloud9 creating](/static/images/compute_and_storage/26_cloud9_creating_environment.png)

When the environment is ready, you can close the "Welcome" tab. You can also 
close the lower pane.

![Cloud9 creating](/static/images/compute_and_storage/27_cloud9_welcome_tab.png)

To open a new terminal window, click on the + button and choose "New Terminal".

![Cloud9 creating](/static/images/compute_and_storage/28_cloud9_open_terminal.png)


## update the OS

Run the following commands to update to the latest packages of Ubuntu.

:::code{showCopyAction=true showLineNumbers=true language=bash}
sudo apt-get update
sudo apt-get upgrade -y
:::


## Clone the workshop repo

We'll need some tools that are stored in the workshop repository. Clone the 
Bioinformatics workshop to your Cloud9 environment.

:::code{showCopyAction=true showLineNumbers=true language=bash}
git clone TBD-REPO
:::


## Expend the volume size

Cloud 9 comes with a default 10 GB volume size which won't be enough for 
running the tools in this workshop.  
In the Cloud9 terminal change to the scripts folder and run the resize script. 
We are going to resize the volume to 20 GB.

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~/environment/bioinformatics-workshop/code/scripts
chmod +x resize.sh
sudo ./resize.sh 20
:::


## Install and run FastQC

Start by installing `openjdk-8-jre`

:::code{showCopyAction=true showLineNumbers=true language=bash}
sudo apt-get install openjdk-8-jre -y
:::

Download FastQC and set it.

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.8.zip
unzip fastqc_v0.11.8.zip
rm fastqc_v0.11.8.zip
chmod +x FastQC/fastqc
PATH=$PATH:/home/ubuntu/FastQC/
:::

Download fastq files and run FastQC

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws s3 cp s3://1000genomes/pilot_data/data/NA12878/pilot3_unrecal/SRR014820_1.fastq.gz .
aws s3 cp s3://1000genomes/pilot_data/data/NA12878/pilot3_unrecal/SRR014820_2.fastq.gz .
fastqc *.gz
:::

Copy the results file to S3 (this will override the files from the previous 
module).

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws s3 cp SRR014820_1_fastqc.zip s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_2_fastqc.zip s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_1_fastqc.html s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_2_fastqc.html s3://aws-biotech-research-files/SRR014820/
:::

You can go back to the S3 console and see that the files time are updated to 
the newly files you've just uploaded.