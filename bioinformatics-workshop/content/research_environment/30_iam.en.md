---
title: "Setup IAM policy and role"
weight: 30
---

In this section you are going to create a set of permissions that will allow 
you to read and write files to the S3 bucket you've previous created. We are 
also allowing read access from public genomics buckets 
(1000genomes and broad-references) to be used by the examples in this workshop.

Use the AWS console search bar to find IAM and then click on IAM.

![search iam](/static/images/compute_and_storage/03_find_iam.png)

On the left side menu click on “Policies”.

![iam menu](/static/images/compute_and_storage/04_iam_menu.png)

Click on the “Create Policy” button.

![iam create policy button](/static/images/compute_and_storage/05_iam_create_policy_button.png)

Click on the JSON tab and then copy & paste the following json code and change 
the “aws-biotech-research-files” to the name of the bucket you’ve created 
before.
:::code{showCopyAction=true showLineNumbers=true language=json}
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "s3:ListBucket"
            ],
            "Resource": [
                "arn:aws:s3:::aws-biotech-research-files",
                "arn:aws:s3:::aws-biotech-research-files/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:PutObject",
                "s3:GetObject",
                "s3:DeleteObject"
            ],
            "Resource": [
                "arn:aws:s3:::aws-biotech-research-files/*"
            ]
        },
        {
            "Effect": "Allow",
            "Action": [
                "s3:Get*",
                "s3:List*"
            ],
            "Resource": [
                "arn:aws:s3:::1000genomes",
                "arn:aws:s3:::1000genomes/*",
                "arn:aws:s3:::broad-references",
                "arn:aws:s3:::broad-references/*"
            ]
        }
    ]
}
:::

When done click on “Next: Tags”.

![iam create policy 1](/static/images/compute_and_storage/06_iam_create_policy_1.png)

Skip the tags creation and click on "Next: Review"

Name the new policy (e.g., biotech-research-bucket-read-write) and optionally 
add a description. 
Click on “Create policy”.

![iam create policy 2](/static/images/compute_and_storage/06_iam_create_policy_2.png)

After the policy has been created click on “Roles” in the left menu and then 
click on “Create role”.

![iam create role button](/static/images/compute_and_storage/07_iam_create_role_button.png)

Click on "EC2" under the common use cases section (we are creating a role that 
is going to be used by an Amazon EC2 instance) and then click on 
"Next: Permissions".

![iam create role 1](/static/images/compute_and_storage/08_create_role_1.png)

In the filter policies search bar type “biotech” or the beginning of *name of* 
the policy you’ve just created and then check the box next to that policy.  
Click “Next: Tags”.

![iam create role 2](/static/images/compute_and_storage/08_create_role_2.png)

Skip the tags creation and click on "Next: Review"

Name the new role (e.g., biotech-research-role) and click on “Create role”.

![iam create role 4](/static/images/compute_and_storage/08_create_role_4.png)
