# Lab 1 - Preparing your lab environment

## Exercise 1: Preparing the lab environment

## Task 1: Ensure the VMs are ready

Hyper-V Integration Services must be installed and running on guest VMs
in order for discovery to identify the apps installed on them.

1.  Login to the provided VM using the credential provided on the **Resources** tab of the Lab interface.

    ![](./media/image61.png)

1.  Open the **Microsoft Edge** from the desktop, then go to the IP
    address of **RHEL-WEB-01**: `192.168.1.24`

    ![A screenshot of a computer Description automatically
    generated](./media/image1.png)

2.  **RHEL-WEB-01** serves a Drupal website that is configured to make
    calls to a database that is hosted on **RHEL-DB-01**. Successfully
    loading the website confirms that both VMs are functioning
    correctly.

## Task 2: Create an Azure Migrate project

1.  In a new Edge tab, navigate to Azure Portal `https://portal.azure.com` , and sign in
    using the credentials provided in the Lab resources

2.  In the Azure portal, in the **Search** box, enter `Azure Migrate`
    then select **Azure Migrate** to go to the Azure Migrate page.

3.  In the left navigation, under **Migration goals**, select **Servers,
    databases and web apps**.

    ![A screenshot of a computer Description automatically
    generated](./media/image2.png)

4.  On the **Servers, databases and web apps** blade, select **Create
    project** in the middle of the page.

5.  On the **Create project** blade, use the following settings to
    create a new project.

    Use the default values for any setting not specified in the table.

    Resource group – select **AZMigrateRG**

    Project - `az-migrate-@lab.LabInstance.Id`

    Geography - **United States**

6.  Select **Create**.

7.  Wait for the deployment to complete before proceeding to the next
    task.

## Task 3: Deploy and configure the Azure Migrate appliance

1.  On the **Servers, databases and web apps** blade, in
    the **Assessment Tools** section, under **Azure Migrate: Discovery and assessment**, select **Discover** and choose **Using appliance**

    ![](./media/image3.png)

2.  On the **Discover** blade, on the **Are your Machines virtualized?** menu, select **Yes, with Hyper-V**.

3.  Under **1. General product key**, in the **Name your
    appliance** box, enter `HV-@lab.LabInstance.Id`, then select **Generate key**.

    >**Note** - The key-generation process can take up to 2 minutes to complete.

4.  Once the key is generated, select the **copy icon** on the **Project
    key** field.

    ![](./media/image4.png)

5.  Under **2. Download Azure Migrate appliance**, select **.zip file.
    500MB**, and *note* the Download button\*.

    <font color=Green>

    > **This would download the PowerShell script that installs the appliance to a Windows Server machine.** 
    
    >For this Lab, the script has **already been downloaded** to the E: drive and **run**. You will **continue past this step**.
    
    </font>

6.  Under **3. Set up the appliance**

7.  Minimize the Edge window, then select the **Azure Migrate Appliance
    Configuration Manager** shortcut on the desktop.

8.  Once the **Azure Migrate Appliance Configuration Manager** page
    loads, you may need to accept the EULA. Select **Accept** if
    prompted.

9.  On the **Azure Migrate Appliance Configuration Manager** page, in
    the **Register Hyper-V appliance by pasting the key here** box,
    paste the key you copied earlier.

10. Select **Verify**.

11. Select **Login**. A modal will appear asking you to **Continue with
    Azure Login**.

12. Select **Copy code & Login** and sign in to your subscription by
    pasting the device code, then selecting your username.

13. When prompted **Are you trying to sign in to Microsoft Azure
    PowerShell?**, select **Continue** and close the newly opened
    browser tab.

14. On the **Azure Migrate Appliance Configuration Manager** page, wait
    for registration to complete.

    ![](./media/image5.png)

    <font color=Green>

    > **It can take up to 10 minutes for registration to complete.** </font>

15. In the **Provide Hyper-V host credentials** section, select **Add
    credentials** and add credentials with the following settings:

    - Friendly Name - `Hypervisor`

    - User Name - `Administrator`

    - Password - `Passw0rd! `

16. In the **Provide Hyper-V host/cluster details** section,
    select **add a discovery source**, then select **Add single
    item** and use the following settings:

    - Discovery source - **Hyper-V Host/Cluster**

    - IP address FQDN - `win-msite54sfl9`

    - Map credentials - **Hypervisor**

17. In the **Provide server credentials to perform software
    inventory** section, ensure that the slider is **enabled**, then add
    credentials with the following settings:

    - Credentials type - **Linux (Non-domain)**

    - Friendly Name - `RHELUser`

    - User Name - `fetch6474`

    - Password - `RHELWorkshop`

18. Select **Start discovery**.

19. Leave Edge open for the next exercise. Discovery will continue
    processing.

# Exercise 3: Create a Business case and run an assessment

## Task 1: Create and review a business case

1.  In Azure portal, go back to the **Azure Migrate Servers, databases
    and web apps** page. Select **Refresh** to verify that your servers
    have been discovered.

    ![A screenshot of a computer Description automatically
    generated](./media/image17.png)

2.  In the **Azure Migrate: Discovery and assessment** section,
    select **Build business case**.

    ![A screenshot of a computer Description automatically
    generated](./media/image18.png)

3.  In the **Build business case** blade, use the following values to
    create the business case.

    - Business case name - `bc-43240741`

    - Target location - **eastus**

    - Migration strategy - **Azure recommended approach to minimize cost**

    - Savings options - **Reserved Instance + Azure Savings Plan**

    - Discount (%) on Pay as you go - **0**

    - Select **Build business case**.

    ![Business Case Refresh](./media/image19.png)

    > The business case can take up to 5 minutes to generate. If it has been more than 5 minutes, select **Refresh**.

4.  On the **bc-43240741** page, review the information indicating Azure readiness and monthly cost estimate for both compute and storage.

## Task 2: Configure, run, and view an assessment

1.  In a new tab navigate **Resource groups** page `https://portal.azure.com/#view/HubsExtension/BrowseResourceGroups.ReactView` select the **AZMigrateRG** resource group and then note down the location of the **Key Vault**, as shown below it is **West US 2**.
   ![](./media/image84.png)

    <font color=Orangered>

    > **Note** - This location would be required to be specified for other resources to be created later in the Lab, also to ensure the **Azure resources are created in the same region** to ensure the Migration is done smoothly.

    </font>

2.  Switch back to the **Azure Migrate** page and under **Azure Migrate: Discovery and assessment** section, select **Assess** and then, in the drop-down menu, select **Azure VM**.

    ![](./media/image20.png)

3.  On the **Create assessment** page, leave the dropdowns menus on
    their defaults.

4.  Select the **Edit** link next to **Assessment settings**,

    ![](./media/image21.png)

5.  On the Assessment Settings page, use the following settings to
    create the assessment.
    <font color=Green>
    > **Accept the default settings for anything not specified in the table.** </font>

    - Target location - **West US 2**

    - Storage type - **Premium managed disks**

    - Savings options - **None**

    - Sizing criteria - **As on premises**

    - VM series - **Dsv3_series**

    - Comfort factor - **1**

    - Offer - **Pay-As-You-Go**

    - Currency - **US Dollar ($)**

    - Discount - **0**

    - VM uptime - **31 Day(s) per month and 24 Hour(s) per day**

    - Already have a Windows Server license? - **No**

    - Security - **No**

6.  Select **Save** to return to Create Assessment, then select **Next:
    Select servers to assess \>**

7.  Use the following settings to create the server group and select the
    servers to be assessed.

    > Accept the default settings for anything not specified in the table.

    Assessment name - `as-43240741`

    Select or create a group - **Create new**

    Group name - `RHEL-Servers`

    List of machines to be added to the group - **RHEL-DB-01** and **RHEL-WEB-01**

8.  Select **Create assessment**. You will be redirected to the **Azure Migrate | Servers, databases and web apps** page.

9.  **Refresh** the page.

10. In the **Azure Migrate: Discovery and Assessment** section, verify that the **Assessments Total** equals **1**, then select **1**.

    ![](./media/image22.png)

11. On the **Azure Migrate: Discovery and Assessment |
    Assessments** page, select the newly created
    assessment **as-43240741**.

    ![](./media/image23.png)

12. On the **as-43240741** page, review the information indicating Azure
    readiness and monthly cost estimate for both compute and storage.

<font color=Green>

> **In real-world scenarios, you should consider installing the Dependency Agent to provide more insights into server dependencies during the
assessment stage.** </font>
