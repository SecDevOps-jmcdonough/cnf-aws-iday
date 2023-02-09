---
title: "Centralized East West"
chapter: true
weight: 4
---


# Centralized East West

For this traffic flow we will focus on the Shared Services, Workload, and Inspection VPCs. Centralized East West is commonly used when there is need for multiple VPCs in the same region to access common private resources such as a shared services VPC, premise items, or workloads/services in other VPCs. The benefit of this design is that this a flexible but simple way to interconnect many resources in the same region. The caveat of this design is traffic will traverse additional AWS networking components for inspection (ie TGW, etc) that will have additional cost.

![](../images/image-cent-eastwest-diag1.png)

**Step 1:** An outbound connection starts with a private EC2 instance initiating a connection to a public resource. The first packet (ie TCP SYN) will be routed to the intrinsic router which will route traffic to the TGW attachment in the same AZ, as configured in the assigned VPC route table. The EC2 instance has a default route, received via DHCP, that points to the first host IP in the subnet which is the intrinsic router.

**Step 2:** The traffic is received at the TGW ENI which then routes the traffic to the Inspection VPC TGW attachment, as configured in the associated Spoke VPC TGW route table.

**Step 3:**  The traffic is received at the TGW ENI in the Inspection VPC which then routes the traffic to the GWLBe endpoint in the same AZ, as configured in the associated VPC route table.

**Step 4:** The traffic is received at the GWLBe endpoint which then routes the traffic to the associated GWLB ENI in the same AZ in the managed Fortinet AWS account/VPC. This is done behind the scene using AWS Private Link.

**Step 5:** The traffic is received at the GWLB ENI and is then encapsulated in a GENEVE tunnel and routed to one of the instances in the FortiGate CNF auto scale group for traffic inspection. Post inspection, if the traffic is allowed, the instance will hairpin the traffic back to the same GWLB ENI over GENEVE. Then the GWLB ENI will hairpin the traffic back to the same GWLBe endpoint.

**Step 6:** The GWLBe endpoint will then route the inspected traffic to the intrinsic router. The intrinsic router will route traffic to the TGW attachment in the same AZ as specified in the VPC route table assigned to the subnet.

**Step 7:** The traffic is received at the TGW ENI which then routes the traffic to the Shared Services VPC TGW attachment, as configured in the associated Inspection VPC TGW route table.

**Step 8:** The traffic is received at the TGW ENI in the Shared Services VPC which then routes the traffic to the destination, as configured in the associated VPC route table.

- 1.  To test out this flow navigate to the **AWS EC2 console and go to Instances > Instances**. Then select **WrkInstance2** and click Connect > EC2 serial console. Copy the instance ID as this will be the username and click connect.

![](../images/image-t5-9.png)

![](../images/image-t5-10.png)

- 2.  Login to the instance with the instance ID as the username and **Fortinet123!** as the password. Then run the commands below to test traffic:

`ping 10.1.2.10`

`ssh ec2-user@10.1.2.10`

`curl -k https://10.1.2.10`

{{% notice note %}}
You are now only allowing HTTPS and RADIUS access between two resourced based on metadata (ie Tags)!
{{% /notice %}}

{{%expand "Question 1: What happens if you try the same test from WrkInstance1?" %}}
You are able to ping but SS annd HTTPS time out.
{{% /expand%}}

{{%expand "Question 2: What tags are allowing this communication to match the dynamic address objects?" %}}
For the ProdAPIBackend object, Tag.env=prod AND Tag.app-role=api AND Tag.app-tier=backend.  For the ProdAuthBackend object, Tag.env=prod AND Tag.app-role=auth AND Tag.app-tier=backend.
{{% /expand%}}

![](../images/image-t5-11.png)
