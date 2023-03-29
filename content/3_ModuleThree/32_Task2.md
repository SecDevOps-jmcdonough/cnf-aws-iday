---
title: "Task 2: Onboard an AWS account to FortiGate CNF"
chapter: true
weight: 2
---


# Task 2: Onboard an AWS account to FortiGate CNF

- 1.  In the FortiGate CNF Console, navigate to AWS Accounts, then click **New** to start the add account wizard.

![](../images/image-t2-1.png)

- 2.  In a new browser tab, log into your AWS account and click on your **IAM user name in the upper right corner**. This will allow you to see and **copy your AWS account ID**.

![](../images/image-t2-2.png)

- 3.  In the FortiGate CNF Console, provide your AWS account ID and select Launch CloudFormation Template. This will redirect you to the CloudFormation Console in your AWS account in the us-west-2 region. **This should complete in 1-2 minutes**.

{{% notice warning %}}
**Note:** Your browser may block the popup window to launch the CloudFormation console. Please check your browser for blocked popup notifications.
{{% /notice %}}

![](../images/image-t2-3.png)

- 4.  This CloudFormation Template creates the following items:

	- 1. S3 bucket for sending logs

	- 2. IAM Cross Account Role which allows us to manage GWLBe endpoints, describe VPCs, push logs to S3, and describe instances and EKS clusters for the SDN connector feature (dynamic address objects based on metadata).

	- 3. Custom resources which kicks off automation on our managed accounts to complete backend tasks for onboarding the AWS account.

- 5.  Please follow through the create stack wizard **without changing the region or any of the parameter values**. Simply follow the steps outlined in the FortiGate CNF Console and click through the CloudFormation wizard.

{{% notice info %}}
**Note:** This CloudFormation template must be ran in the us-west-2 (Oregon) region for successful onboarding and ongoing operations of this AWS account with FortiGate CNF.
{{% /notice %}}

![](../images/image-t2-4.png)

- 6.  Once the CloudFormation template has been created successfully in the CloudFormation Console, in the FortGate CNF console click **OK** you should see your account showing **Success** in the AWS account page of FortiGate CNF.

![](../images/image-t2-5.png)

- 7.  This concludes this section.
