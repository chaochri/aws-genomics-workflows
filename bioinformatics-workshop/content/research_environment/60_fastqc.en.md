---
title: "Setup FastQC"
weight: 60
---

Update the current installed packages to the lastet version by running the 
following command.

:::code{showCopyAction=true showLineNumbers=true language=bash}
sudo yum update -y
:::

FasqtQC requires the Java runtime to execute. Install JRE with the following 
commands.

:::code{showCopyAction=true showLineNumbers=true language=bash}
sudo amazon-linux-extras enable corretto8
sudo yum install java-1.8.0-amazon-corretto -y
:::

Download and install FastQC

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~
wget https://www.bioinformatics.babraham.ac.uk/projects/fastqc/fastqc_v0.11.8.zip
unzip fastqc_v0.11.8.zip
rm fastqc_v0.11.8.zip
chmod +x FastQC/fastqc
PATH=$PATH:/home/ec2-user/FastQC/
:::

Download FASTQ files and run analysis

:::code{showCopyAction=true showLineNumbers=true language=bash}
cd ~
aws s3 cp s3://1000genomes/pilot_data/data/NA12878/pilot3_unrecal/SRR014820_1.fastq.gz .
aws s3 cp s3://1000genomes/pilot_data/data/NA12878/pilot3_unrecal/SRR014820_2.fastq.gz .
fastqc *.gz
:::

You can upload the result files to your S3 bucket. Change the 
"aws-biotech-research-files" to the S3 bucket name you've created.

:::code{showCopyAction=true showLineNumbers=true language=bash}
aws s3 cp SRR014820_1_fastqc.zip s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_2_fastqc.zip s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_1_fastqc.html s3://aws-biotech-research-files/SRR014820/
aws s3 cp SRR014820_2_fastqc.html s3://aws-biotech-research-files/SRR014820/
:::

You can now navigate to the AWS S3 console (search for S3 in the search bar) 
and navigate to your bucket. You should see a folder named SRR014820 with the 
files you’ve uploaded. You can check the box to one of the html files and then 
click on “Actions” → “Open”. This will open the html file and you will be able 
to see the generated report.

![html file](/static/images/compute_and_storage/18_open_s3_html_file.png)