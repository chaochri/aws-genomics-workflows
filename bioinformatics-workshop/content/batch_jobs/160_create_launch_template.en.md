---
title: "Create a Launch Template"
weight: 160
---

Use the AWS console search bar to find Launch Templates and then click on 
Launch Templates.

![find LT](/static/images/batch_jobs/10_find_launch_templates.png)

Click on “Create launch template”.

![create LT button](/static/images/batch_jobs/11_create_launch_template_button.png)

**Launch template name** \- Give a name to the launch template 
(e.g., batch-bioinformatics-launch-template)  

![create LT 1](/static/images/batch_jobs/12_create_launch_template_1.png)

**Storage (Volumes)** \- Click on "Add new volume" and select /dev/xvda 
(this is the instance root volume).  
**Size (GiB)** \- Enter 100 (or any size that will be enough for you workload).  
**Encrypted** \- Select "Yes" to have an encrypted volume.

![create LT 2](/static/images/batch_jobs/13_create_launch_template_2.png)

**Under Resource tags** \- Add the same tags as before (e.g., Environment - Dev, 
Project - Biotech).

![create LT 3](/static/images/batch_jobs/14_create_launch_template_3.png)

**Under Advanced details - User Data** \- Add the following code. This code will run once when the 
instance is launched and will install the awscli v2 to be used from inside the 
containers later on, configure ECS default behavior, setup system manager, and 
set up CloudWatch logs.

When done, click on "Create launch template".

:::code{showCopyAction=true showLineNumbers=true language=bash}
MIME-Version: 1.0
Content-Type: multipart/mixed; boundary="==BOUNDARY=="

--==BOUNDARY==
Content-Type: text/cloud-config; charset="us-ascii"

#cloud-config
repo_update: true
repo_upgrade: security

packages:
- jq
- btrfs-progs
- sed
- git
- amazon-ssm-agent
- unzip
- amazon-cloudwatch-agent

write_files:
- permissions: '0644'
  path: /opt/aws/amazon-cloudwatch-agent/etc/config.json
  content: |
    {
      "agent": {
        "logfile": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log"
      },
      "logs": {
        "logs_collected": {
          "files": {
            "collect_list": [
              {
                "file_path": "/opt/aws/amazon-cloudwatch-agent/logs/amazon-cloudwatch-agent.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/amazon-cloudwatch-agent.log"
              },
              {
                "file_path": "/var/log/cloud-init.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/cloud-init.log"
              },
              {
                "file_path": "/var/log/cloud-init-output.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/cloud-init-output.log"
              },
              {
                "file_path": "/var/log/ecs/ecs-init.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/ecs-init.log"
              },
              {
                "file_path": "/var/log/ecs/ecs-agent.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/ecs-agent.log"
              },
              {
                "file_path": "/var/log/ecs/ecs-volume-plugin.log",
                "log_group_name": "/aws/ecs/container-instance/${Namespace}",
                "log_stream_name": "/aws/ecs/container-instance/${Namespace}/{instance_id}/ecs-volume-plugin.log"
              }
            ]
          }
        }
      }
    }

runcmd:

# start the amazon-cloudwatch-agent
- /opt/aws/amazon-cloudwatch-agent/bin/amazon-cloudwatch-agent-ctl -a fetch-config -m ec2 -s -c file:/opt/aws/amazon-cloudwatch-agent/etc/config.json

# install aws-cli v2 and copy the static binary in an easy to find location for bind-mounts into containers
- curl -s "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "/tmp/awscliv2.zip"
- unzip -q /tmp/awscliv2.zip -d /tmp
- /tmp/aws/install -b /usr/bin

# check that the aws-cli was actually installed. if not shutdown (terminate) the instance
- command -v aws || shutdown -P now

- mkdir -p /opt/aws-cli/bin
- cp -a $(dirname $(find /usr/local/aws-cli -name 'aws' -type f))/. /opt/aws-cli/bin/

# set environment variables for provisioning
- export GWFCORE_NAMESPACE=${Namespace}
- export INSTALLED_ARTIFACTS_S3_ROOT_URL=$(aws ssm get-parameter --name /gwfcore/${Namespace}/installed-artifacts/s3-root-url --query 'Parameter.Value' --output text)

# enable ecs spot instance draining
- echo ECS_ENABLE_SPOT_INSTANCE_DRAINING=true >> /etc/ecs/ecs.config

# pull docker images only if missing
- echo ECS_IMAGE_PULL_BEHAVIOR=prefer-cached >> /etc/ecs/ecs.config

- cd /opt
- aws s3 sync $INSTALLED_ARTIFACTS_S3_ROOT_URL/ecs-additions ./ecs-additions
- chmod a+x /opt/ecs-additions/provision.sh
- /opt/ecs-additions/provision.sh

--==BOUNDARY==--
:::

![create LT 4](/static/images/batch_jobs/15_create_launch_template_4.png)