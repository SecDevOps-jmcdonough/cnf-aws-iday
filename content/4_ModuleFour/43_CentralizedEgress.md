---
title: "Centralized Egress"
chapter: true
weight: 3
---


# Centralized Egress

For this traffic flow we will focus on the Shared Services, Workload, and Inspection VPCs. Centralized egress is commonly used when there is a strong desire to control egress traffic through a common set of NAT GWs in an egress or what we call an Inspection VPC. The benefit of this design is that you only need NAT GWs in the Inspection VPC (one per AZ) vs every VPC (one per AZ), which have an hourly cost. The caveat of this design is traffic will traverse additional AWS networking components for inspection (ie TGW, etc) that will have additional cost.

![](../images/image-cent-egress-diag1.png)

**Step 1:** An outbound connection starts with a private EC2 instance initiating a connection to a public resource. The first packet (ie TCP SYN) will be routed to the intrinsic router which will route traffic to the TGW attachment in the same AZ, as configured in the assigned VPC route table. The EC2 instance has a default route, received via DHCP, that points to the first host IP in the subnet which is the intrinsic router.

**Step 2:** The traffic is received at the TGW ENI which then routes the traffic to the Inspection VPC TGW attachment, as configured in the associated Spoke VPC TGW route table.

**Step 3:**  The traffic is received at the TGW ENI in the Inspection VPC which then routes the traffic to the GWLBe endpoint in the same AZ, as configured in the associated VPC route table.

**Step 4:** The traffic is received at the GWLBe endpoint which then routes the traffic to the associated GWLB ENI in the same AZ in the managed Fortinet AWS account/VPC. This is done behind the scene using AWS Private Link.

**Step 5:** The traffic is received at the GWLB ENI and is then encapsulated in a GENEVE tunnel and routed to one of the instances in the FortiGate CNF auto scale group for traffic inspection. Post inspection, if the traffic is allowed, the instance will hairpin the traffic back to the same GWLB ENI over GENEVE. Then the GWLB ENI will hairpin the traffic back to the same GWLBe endpoint.

**Step 6:** The GWLBe endpoint will then route the inspected traffic to the intrinsic router. The intrinsic router will route traffic directly to the NAT GW in the same AZ as specified in the VPC route table assigned to the subnet.

**Step 7:** The traffic is received at the NAT GW ENI which then routes the traffic to the intrinsic router. The intrinsic router will route traffic directly to the IGW as specified in the VPC route table assigned to the subnet.

**Note:** The NAT GW will source NAT the traffic to the private IP assigned to its ENI.

**Step 8:** The destination will receive the traffic and respond. The return traffic will follow these steps in reverse.

{{% notice note %}}
The IGW will source NAT the traffic to the public EIP assigned to the NAT GW ENI.
{{% /notice %}}

- 1.  To test out this flow navigate to the **AWS EC2 console and go to Instances > Instances**. Then select either **WrkInstance** and click Connect > EC2 serial console. Copy the instance ID as this will be the username and click connect.

![](../images/image-t5-6.png)

![](../images/image-t5-7.png)

- 2.  Login to the instance with the instance ID as the username and **Fortinet123!** as the password. Then run the commands below to test traffic:

`ping 8.8.8.8`

`curl http://ipinfo.io`

`curl https://ipinfo.io`

{{% notice note %}}
You are now only allowing HTTPS outbound to one FQDN and ICMP to any public IP within the United States!
{{% /notice %}}

{{%expand "Question 1: What happens if you try the same test from SSInstance1?" %}}
You should be able to access the IPinfo.io site over HTTPS and ping any public IP within the United States.{{% /expand%}}


{{%expand "Question 2: What address objects are allowing this communication to work even though the sdn-group = group3 for this instance?"%}}
AppPublicSubnet1 + AppPublicSubnet2.  Remember that Dynamic, FQDN, and standard address objects still resolve to IPs.  Since the Application-VPC and SharedServices-VPC share the same CIDR the data plane traffic will match on those Address objects.  

A solution to this would be to use multiple CNF Instances in a region or expand on your tagging strategy to make the objects be more specific while avoiding using broad subnet CIDR values in the same L4 rule.{{% /expand%}}


![](../images/image-t5-8.png)
