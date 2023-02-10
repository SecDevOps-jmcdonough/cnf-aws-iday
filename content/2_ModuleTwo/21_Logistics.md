---
title: "Workshop Logistics"
chapter: true
weight: 1
---


# Workshop Logistics

## Accessing an AWS environment

For the AWS Immersion Days, we will provide you the following via email on the day of the event:

  * **AWS sign in link**
  * **IAM User w/ console access**
  * **Password for the IAM User**

{{% notice note %}}
If you are wanting to run through this in your own environment for training purposes, use the Master CFT to deploy the base environment: [**https://hacorp-us-east-1.s3.amazonaws.com/forticnf-aws-workshop-cloudformation/Master_CNF_DualAZ.template.json**](https://hacorp-us-east-1.s3.amazonaws.com/forticnf-aws-workshop-cloudformation/Master_CNF_DualAZ.template.json)
{{% /notice %}}

## Accessing the FortiGate CNF Console

FortiGate CNF and other SaaS solutions are tied to your [**FortiCloud**](https://support.fortinet.com/). If you do not already have one, please navigate [**here**](https://support.fortinet.com/cred/#/sign-up) and complete the registration process.

If you already have an account and don't want to use that for this lab, it is recommended to create your own FortiCloud account.

Once logged in, you will see your FortiCloud dashboard.

*You will log into the FortiGate CNF console later during the hands on section.*

{{% notice info %}}
Please **log out** before proceeding to the next part of the workshop.
{{% /notice %}}

![](../images/image-forticloud.png)

## Navigating the AWS Console

When you first login you will see the Console Home page.

Use the **Search Box** at the top to search for services such as EC2, VPC, CloudFormation, etc.

When the results pop up, **right click** the name of the service and open the desired console in a new tab. This makes navigation easier.

![](../images/image-awsconsole1.png)

![](../images/image-awsconsole2.png)
