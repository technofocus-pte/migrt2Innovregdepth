**Lab 5 - Defender for Cloud & Sentinel.**

## Task 1: Enable JIT on your VMs from Microsoft Defender for Cloud

1.  Type **Microsoft Defender for Cloud** in the Azure portal search
    box, then navigate and click on **Microsoft Defender for Cloud**
    under **Services**.
![](./media/mage1.png)
<img src="./media/image1.png"
style="width:6.19878in;height:3.12544in" />

2.  On **Microsoft Defender for Cloud \| Overview** page left-sided
    pane, navigate to the **Cloud Security** section and then click on
    **Workload Protections**.

<img src="./media/image2.png" style="width:6.5in;height:4.44306in"
alt="A screenshot of a computer Description automatically generated" />

3.  On **Microsoft Defender for Cloud \| Workload protections** page,
    scroll down and click on **Just-in-time VM access** under the
    **Advanced protection** section as shown in the below image.

<img src="./media/image3.png" style="width:6.5in;height:2.56528in"
alt="A screenshot of a computer Description automatically generated" />

4.  On the **Just-in-time VM access** page, navigate to the **Virtual
    machines** section and click on **Non Configured** tab. You will see
    the VMs – **PostgreSrv** is listed under the **Non** **Configured**
    tab.

<img src="./media/image4.png" style="width:6.5in;height:4.825in"
alt="A screenshot of a computer Description automatically generated" />

5.  Select the VMs and click on **Enable JIT on 2 VMs** button on the
    right side.

<img src="./media/image5.png" style="width:6.5in;height:2.47917in"
alt="A screenshot of a computer Description automatically generated" />

6.  On the **JIT VM access configuration** page, click on **Save**.

<img src="./media/image6.png" style="width:6.5in;height:1.82361in"
alt="A screenshot of a computer Description automatically generated" />

7.  You’ll receive a notification - **Just-in-time VM access
    configuration has started**.

<img src="./media/image7.png" style="width:3.09418in;height:0.92721in"
alt="A screenshot of a computer Description automatically generated" />

8.  Click on the **Configured** tab under the **Virtual machines**
    section, you will see that the VMs **PostgreSrv** is listed under
    the **Configured** tab.

<img src="./media/image8.png" style="width:6.5in;height:3.41597in"
alt="A screenshot of a computer Description automatically generated" />

9.  Now in order to connect to this VM, the access is granted upon
    request.

<img src="./media/image9.png" style="width:6.5in;height:4.42222in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Generating and Investigating Security alerts

1.  In **Microsoft Defender for Cloud** under **General** section select
    **Security alerts**

<img src="./media/image10.png" style="width:6.5in;height:2.42986in" />

2.  Click on the Sample alerts button to generate alerts.

<img src="./media/image11.png" style="width:6.5in;height:2.42986in"
alt="A screenshot of a computer Description automatically generated" />

3.  Click on **Create sample alerts** button.

<img src="./media/image12.png" style="width:6.5in;height:6.56111in"
alt="A screenshot of a computer Description automatically generated" />

<img src="./media/image13.png" style="width:3.17753in;height:2.26073in"
alt="A screenshot of a computer error Description automatically generated" />

4.  Sample Alerts will be generated

<img src="./media/image14.png" style="width:3.03167in;height:2.22948in"
alt="A screenshot of a computer Description automatically generated" />

5.  Click on **Refresh** button, and you will be able to see the Sample
    alerts.

<img src="./media/image15.png" style="width:6.5in;height:3.14583in"
alt="A screenshot of a computer Description automatically generated" />

6.  You can click on any alert that you wish to investigate

7.  On the alert overview pane, investigate the following details.

    1.  Severity, Status, and Activity time

    2.  Description that explains the precise activity that was detected

    3.  Affected resources

    4.  Kill chain intent of the activity on the MITRE ATT&CK matrix.

8.  For more detailed information that can help you to investigate the
    suspicious activity, click on **View full details** button.

<img src="./media/image16.png" style="width:6.5in;height:3.55764in"
alt="A screenshot of a computer Description automatically generated" />

9.  Review the information under **Alert details** tab.

<img src="./media/image17.png" style="width:6.5in;height:5.05625in" />

10. Click on the **Take action** button and review the available options

<img src="./media/image18.png" style="width:6.5in;height:3.64792in"
alt="A screenshot of a computer Description automatically generated" />

# Exercise 2 – Deploying Sentinel

## Task 1: The Microsoft Sentinel workspace

In this exercise we will see how to create a Microsoft Sentinel
workspace.

1.  Navigate to the **http://portal.azure.com** and log in with the
    **MOD Administrator** credentials provided on the home tab of your
    lab environment.

2.  In the top search bar, type **Microsoft Sentinel** and click on
    **Microsoft Sentinel**.

<img src="./media/image19.png" style="width:6.26806in;height:4.00278in"
alt="A screenshot of a computer Description automatically generated" />

3.  In the **Microsoft Sentinel** screen, click **Create** at the top
    left.

<img src="./media/image20.png" style="width:6.26806in;height:4.78847in"
alt="A screenshot of a computer Description automatically generated" />

4.  You can choose to add **Microsoft Sentinel** to an existing **Log
    Analytics** **workspace** or build a new one. We will create a new
    one, so click on **Create a new workspace**.

<img src="./media/image21.png" style="width:6.26806in;height:4.72917in"
alt="A screenshot of a computer Description automatically generated" />

5.  In the **Create Log Analytics workspace** page, fill out the form as
    follows:

    1.  Subscription: **Azure Pass - Sponsorship**

    2.  Resource Group: click on **Create new** **LAWResourceGroup**

    3.  Workspace Name: **SentWrkspcXXXXXX** \[substitute **XXXXXX**
        with random number\]

    4.  Region: **West US**

    5.  Click **Review + create**.

<img src="./media/image22.png"
style="width:6.49994in;height:6.04583in" />

6.  Click **Create** after the validation is completed. The creation
    takes a few seconds.

<img src="./media/image23.png" style="width:6.5in;height:5.81389in"
alt="A screenshot of a computer Description automatically generated" />

7.  You will be redirected back to the **Add Microsoft Sentinel** to a
    workspace page, click on the **Refresh** button.

<img src="./media/image24.png" style="width:6.26806in;height:4.47639in"
alt="A screenshot of a computer Description automatically generated" />

8.  Select your workspace and click **Add** at the bottom.

<img src="./media/image25.png" style="width:6.5in;height:6.08542in"
alt="A screenshot of a computer Description automatically generated" />

9.  You should receive notification as shown in below image

<img src="./media/image26.png" style="width:4.58397in;height:0.8647in"
alt="A screenshot of a computer Description automatically generated" />

10. Your Microsoft Sentinel workspace is now ready to use!

<img src="./media/image27.png" style="width:6.5in;height:3.47847in"
alt="A screenshot of a computer Description automatically generated" />

## Task 2: Enable Data connectors.

This exercise shows you how to enable Data connectors.

1.  In the browser tab navigate to
    <https://portal.azure.com/#view/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/AllUsers>
    and select the Tenant Administrator account

<img src="./media/image28.png" style="width:6.26806in;height:2.50278in"
alt="A screenshot of a computer Description automatically generated" />

2.  Select the **Assigned roles** under Manage and then click on **+ Add
    assignments.**

<img src="./media/image29.png"
style="width:6.26806in;height:3.18958in" />

3.  Search and select **Security Administrator**, then click on the
    **Add** button.

<img src="./media/image30.png"
style="width:6.26806in;height:4.86319in" />

<img src="./media/image31.png" style="width:3.46923in;height:0.55216in"
alt="A close up of a text Description automatically generated" />

4.  On the Azure Portal
    [**http://portal.azure.com**](http://portal.azure.com) and search
    for **Microsoft Sentinel** and click on **Microsoft Sentinel**.

<img src="./media/image32.png" style="width:5.82373in;height:2.83373in"
alt="A screenshot of a computer Description automatically generated" />

5.  Select **SentWrkspc123456**.

<img src="./media/image33.png" style="width:6.08418in;height:2.91707in"
alt="A screenshot of a computer Description automatically generated" />

6.  Now select **Data Connectors** under **Configuration** section.

<img src="./media/image34.png" style="width:2.38542in;height:4.92986in"
alt="A screenshot of a computer Description automatically generated" />

7.  You should get the message “Data Connector with "content source =
    gallery content" have been removed. In that message select the
    **Click here** link

<img src="./media/image35.png"
style="width:6.26766in;height:3.38542in" />

8.  On the **Out-of-the-box Content Centralization** page click on
    **Continue**

<img src="./media/image36.png"
style="width:6.26806in;height:3.60069in" />

9.  Click on the **Complete centralization** button

<img src="./media/image37.png" style="width:6.5in;height:4.18472in"
alt="A screenshot of a computer Description automatically generated" />

10. You should receive the notification as shown in below image

<img src="./media/image38.png" style="width:2.00028in;height:0.65634in"
alt="A close-up of a sign Description automatically generated" />

11. From the top click on the link for **Microsoft Sentinel** or
    navigate back to the Sentinel page.

<img src="./media/image39.png" style="width:6.26806in;height:0.825in"
alt="A screen shot of a computer Description automatically generated" />

12. Click on the **Refresh** button and you should be able to see the
    Data connectors showing.

<img src="./media/image40.png" style="width:6.5in;height:4.36875in"
alt="A screenshot of a computer Description automatically generated" />

13. Click on **Content hub**

<img src="./media/image41.png" style="width:6.26806in;height:3.25662in"
alt="A screenshot of a computer Description automatically generated" />

14. On the Content hub page search for **Azure Activity** and then
    select **Azure Activity** content and click on **Install** button

<img src="./media/image42.png" style="width:6.5in;height:3.5875in"
alt="A screenshot of a computer Description automatically generated" />

15. On the Content hub page search for **Microsoft defender for cloud**
    and then select **Microsoft Defender for Cloud** content and click
    on **Install** button

<img src="./media/image43.png" style="width:6.5in;height:3.57361in"
alt="A screenshot of a computer Description automatically generated" />

## Task 3: Enable Azure Activity data connector

This exercise shows you how to enable the Azure Activity data connector.
This connector will bring all the audit events, for actions performed in
your Azure subscription, into your Microsoft Sentinel workspace.

While still on the **Microsoft Sentinel** page click on **Data
Connectors** under **Configuration** section.

<img src="./media/image44.png" style="width:6.26806in;height:3.02011in"
alt="A screenshot of a computer Description automatically generated" />

In the data connectors screen, type **actvity** in the search bar,
select the **Azure Activity** connector and click on **Open connector
page**.

<img src="./media/image45.png" style="width:6.26806in;height:2.95328in"
alt="A screenshot of a computer Description automatically generated" />

On the **Azure Activity connector** page, go to option number **2.
Connect your subscriptions through diagnostic settings new pipeline**.
This method leverages Azure Policy and it brings many improvements
compared to the old method (more details about these improvements can be
found here). Click on the **Launch Azure Policy Assignment** wizard,
this will redirect you to the policy creation page.

<img src="./media/image46.png" style="width:6.26806in;height:3.41597in"
alt="A screenshot of a computer Description automatically generated" />

On the **Scope** selection select **Azure Pass – Sponsorship** and under
**Resource Group** select **LAWResourceGroup**. Click **Select**.

<img src="./media/image47.png" style="width:6.26806in;height:3.49861in"
alt="Screens screenshot of a computer Description automatically generated" />

Go to the **Parameters** tab. On the **Primary Log Analytics workspace**
select the **MicrosoftSentinelWorkspace**.

<img src="./media/image48.png" style="width:6.26806in;height:4.32431in"
alt="A screenshot of a computer Description automatically generated" />

Under **Remediation** tab, select the check box besides **Create a
remediation task** and then click on **Review + create** button

<img src="./media/image49.png" style="width:6.26806in;height:4.29861in"
alt="A screenshot of a computer screen Description automatically generated" />

On the **Review + create** tab, click on the **Create** button.

<img src="./media/image50.png" style="width:6.26806in;height:4.68264in"
alt="A screenshot of a computer Description automatically generated" />

In the **Notification** pane you will be able to see the ‘**Role
Assignments creation succeeded**’, ‘**Remediation task creation
succeeded**’ and ‘**Creating policy assignment succeeded**’
notifications.

<img src="./media/image51.png" style="width:5.5in;height:4.29097in"
alt="A screenshot of a computer Description automatically generated" />

On the **Azure Activity connector** page you will be able to see the
connection status.

<span class="mark">**Note**: It is normal if you don't immediately see
the connector showing as connected and in green, it takes around 30
minutes for the process to complete. Also, each subscription has a
maximum of 5 destinations for its activity logs. If this limit is
already reached, the policy created as part of this exercise won't be
able to add an additional destination to your Microsoft Sentinel
workspace.</span>

<img src="./media/image52.png" style="width:6.26806in;height:3.39514in"
alt="A screenshot of a computer Description automatically generated" />

Continue to the next exercise then you can check back after 30 minutes.

## Task 4: Enable Microsoft Defender for Cloud data connector.

This exercise shows you how to enable the Microsoft Defender for Cloud
data connector. This connector allows you to stream your security alerts
from Microsoft Defender for Cloud into Microsoft Sentinel, so you can
view Defender data in workbooks, query it to produce alerts, and
investigate and respond to incidents.

While still on the **Microsoft Sentinel** page click on **Data
Connectors** under **Configuration** section.

<img src="./media/image44.png" style="width:6.26806in;height:3.01944in"
alt="A screenshot of a computer Description automatically generated" />

In the **Data connectors** screen, type **tenant** in the search bar,
select the **Tenant-based Microsoft Defender for Cloud** **(Preview)**
connector and click on **Open connector page**.

<img src="./media/image53.png"
style="width:6.26806in;height:3.22639in" />

On the **Tenant-based Microsoft Defender for Cloud** **(Preview)**
connector page, under **Configuration** section click on the **Connect**
button.

<img src="./media/image54.png" style="width:6.26806in;height:3.11234in"
alt="A screenshot of a computer Description automatically generated" />

You should receive the notification as Connected successfully.

<img src="./media/image55.png"
style="width:6.26806in;height:5.0625in" />

Wait for 1-2 minutes and then refresh the page, the Status of the
connector should also be updated to **Connected.**

<img src="./media/image56.png"
style="width:6.26806in;height:3.11528in" />

# Exercise 3- Integration
