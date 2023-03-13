---
title: "Task 2: FortiManager Integration"
chapter: true
weight: 2
---


# Task 2: FortiManager Integration

- 1.  In the AWS CloudFormation console, make sure you are in the **us-west-1 (N. California)** region, then **toggle the view nested button to off** > then select the stack name > and on the details pane select the outputs tab. You should see the output for **FortiManagerLoginURL**. Login to the FortiManager with the username **`admin`** and the password **`FORTInet123!`**.

![](../images/image-t6-10.png)

- 2. In the FortiManager GUI for the first login, you will be prompted to complete the setup wizard. Click **Begin**, **Next**, and **Finish**. Then on the home page, select **Device Manager**.

{{% notice info %}}
**Note:** By default, FortiManager will be deployed with a self-signed certificate. You will see browser warnings for this and need to accept this to login successfully.
{{% /notice %}}

![](../images/image-t6-11.png)

![](../images/image-t6-12.png)

- 3. At the top left, click **Add Device** and select **Discover Device**.

![](../images/image-t6-13.png)

![](../images/image-t6-14.png)

- 4.  In the FortiGate CNF console, navigate to CNF instances and **select and edit** the CNF instance, then toggle **Display Primary FortiGate Information**. Copy the Primary FGT information and click **Exit**.

![](../images/image-t6-15.png)

- 4.  In the FortiManager GUI, enter in the Primary FGT IP, toggle on **Use Legacy Device Login**, fill out the rest and click **Next**.

![](../images/image-t6-16.png)

- 5.  On the next page, click **Next** and the FortiManager will begin discovering and adding the CNF Instance.

![](../images/image-t6-17.png)

- 6.  Once this process is complete, click **Import Now**, then **Import Policy Package** and click **Next**. Select **Automatically import all VDOMs** and click **Next** and the FortiManager will import a policy package for each VDOM. Once complete, click **Finish**.

![](../images/image-t6-18.png)

![](../images/image-t6-19.png)

![](../images/image-t6-20.png)

![](../images/image-t6-21.png)

- 7.  In the top left, click **Device Manager** then click **Policy & Object**. Then **grab this bar and drag it to the right** to see the full names of the policy packages.

![](../images/image-t6-22.png)

![](../images/image-t6-23.png)

- 8.  Then **right click** the policy package ending in **...root** and click **Delete**. Do the same for the **default** policy package as well. Now you should be left with the policy package ending in **...FG-Traffic**.

![](../images/image-t6-24.png)

![](../images/image-t6-25.png)

- 9.  Select **policy #1**, click **Edit**, then **Edit** again in the drop down menu.

![](../images/image-t6-26.png)

- 10.  In the edit firewall policy page, change the **name to Inbound**, set the **Destination to be RFC1918-GRP**, select **default for Application Control**, then **all_default for IPS**, toggle **Log Allowed Traffic** to **All Sessions**, finally type a note in the **Change Note** text box, then click **OK** to apply the changes.

![](../images/image-t6-27.png)

![](../images/image-t6-28.png)

![](../images/image-t6-29.png)

- 11.  **Right click policy #1** again and click **Clone Reverse** to create a new policy. Then select **policy #2**, click **Edit**, then **Edit** again in the drop down menu.

![](../images/image-t6-30.png)

![](../images/image-t6-31.png)

- 12.  In the edit firewall policy page, change the **name to Outbound**, select **default for AntiVirus Profile**, then **monitor-all for Web Filter Profile**, then **deep-inspection for SSL/SSH Inspection**, finally type a note in the **Change Note** text box, then click **OK** to apply the changes.

![](../images/image-t6-32.png)

![](../images/image-t6-33.png)

![](../images/image-t6-34.png)

- 13.  **Right click** the policy package and select **Install Wizard**. Then click **Next**, **Next**, **Install**. The FortiManager will begin pushing the modified policy package to the CNF Instance. Once complete, click **Finish**.

![](../images/image-t6-35.png)

![](../images/image-t6-36.png)

![](../images/image-t6-37.png)

![](../images/image-t6-38.png)

![](../images/image-t6-39.png)

![](../images/image-t6-40.png)

- 14.  This concludes this section.
