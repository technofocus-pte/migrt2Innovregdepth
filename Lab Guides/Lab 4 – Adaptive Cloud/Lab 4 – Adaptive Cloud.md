# Lab 4 – Adaptive Cloud

## Objective

In this lab we will manage resources across environments with Azure Arc,
you can secure, monitor, and govern infrastructure across your
environments, including on-premises, other public clouds, and edge
devices.

## Task 1 – Setup the On-premise machine 

1.  Open the Edge browser on the Lab VM and navigate to the link to download the **Windows Server 2022 image** - ```https://strg4vmimages2024.blob.core.windows.net/images/WinSrv20224Arc.zip```

2.  Once it is downloaded click on the **Open file** link

    ![](./media/image1.png)

3.  Click on Extract all button.

    ![](./media/image2.png)

4.  Provide the folder as ```E:\Virtual Machines``` and then click
    on the **Extract** button.

    ![](./media/image3.png)

5.  Open the **Hyper-V Manager** from the Task bar and then right click
    on the Server name and then choose **Hyper-V Setting**

    ![](./media/image4.png)

6.  On the **Settings** window, choose **Enhanced Session Mode Policy**
    option and then enable the check box for **Allow enhanced session
    mode** and then click on the **OK** button.

    ![](./media/image5.png)

7.  In the **Hyper-V Manager** click on the **Import Virtual Machine**
    action.

    ![](./media/image6.png)

8.  On the **Locate Folder** page, click on the **Browse** button,
    browse to ```E:\Virtual Machines\WinSrv20224Arc``` and then
    click on the **Select Folder** button.

    ![](./media/image7.png)

9.  On the **Locate Folder** page, click on the **Next** button.

    ![](./media/image8.png)

10. On the **Select Virtual Machine** page, click on the **Next**
    button.

11. On the **Choose Import type** page, leave the default option and
    then click on the **Next** button.

    ![](./media/image9.png)

12. On the **Connect Network** page, from the **Connection** drop-down
    choose the **Microsoft Hyper-V Network Adapter** and then click on
    the **Next** button.

    ![](./media/image10.png)

13. On the **Complete Import Wizard** page review the detail and then
    click on the **Finish** button.

    ![](./media/image11.png)

14. Right click on the **WinSrv20224Arc** VM and then choose the
    **Start** option

    ![](./media/image12.png)

15. Again, right click on the **WinSrv20224Arc** VM and then choose the
    **Connect** option

    ![](./media/image13.png)

16. Press the **Ctrl+Alt+Delete** button from the Virtual Machine
    Connection window

    ![](./media/image14.png)

17. Login with the below credentials

    1.  Username – Administrator

    2.  Password – ```P@55w.rd1234```

    ![](./media/image15.png)

18. Ensure that you have successfully logged in.

## Task 2 – Add Azure Arc resource via Script

1.  In a new Edge tab, go to ``` https://portal.azure.com/```, and
    sign in using the provided credentials.

2.  While in the Azure Portal in the search type ```arc``` and then select Azure Arc

    ![](./media/image16.png)

3.  Under **Manage resources across environments** click on the **Add
    resources** button.

    ![](./media/image17.png)

4.  On the **Add Azure Arc resources** page, click on **Add/Create**
    button and choose **Add a machine.**

    ![](./media/image18.png)

5.  On the **Add servers with Azure Arc** page, click on **Generate
    script** button under the **Add a single server**.

    ![](./media/image19.png)

6.  On the **Add a server with Azure Arc** page, provide the below
    details.

    *  Choose the Region first Region - **West US**

    *  Resource group – Click on **Create new**  ```RG4ArcVM```

    *  Operating System – **Windows**

    *  Connect SQL Server – **Uncheck the box**

    *  Click on the **Download and run script button**

        ![](./media/image20.png)

7.  Click on the Download button and also click on the Copy button.

    ![](./media/image21.png)

8.  Switch back to the **WinSrv20224Arc** VM and right-click on the
    Start button and select **Windows PowerShell (Admin)**

    ![](./media/image22.png)

9.  On the **Windows PowerShell (Admin)** Window paste the copied script
    from clipboard

    ![](./media/image23.png)

10. The Script should be launched as shown in below image

    ![](./media/image24.png)

11. When prompted to login, login with the provided credentials

    ![](./media/image25.png)

12. Once authentication is successful switch back to the PowerShell
    window

    ![](./media/image26.png)

13. The Machine should be successfully connected to Azure

    ![](./media/image27.png)

## Task 3 – Manage the Arc Server

1.  Switch back to the Azure Portal windows in the Edge browser.

2.  Click on the Azure Arc link

    ![](./media/image28.png)

3.  Select **Machines** under Azure Arc resources

    ![](./media/image29.png)

4.  You should be able to see the Server showing as **Connected**

    ![](./media/image30.png)

5.  Click on the Server name to open the details, the from the
    **Overview** page, click on **Updates**

    ![](./media/image31.png)

6.  From the **Periodic assessment** drop-down choose **Enable** and
    then click on **Save** button.

    ![](./media/image32.png)

    ![](./media/image33.png)

7.  Back on the **Overview** page, click on **Monitoring insights**

    ![](./media/image34.png)

8.  On the Insights page, click on the **Enable** button.

    ![](./media/image35.png)

9.  On the Monitoring configuration page, click on Configure button.

    ![](./media/image36.png)

    >**Note** – The monitoring resource can take around 5-10 minutes to be deployed.


    ![](./media/image37.png)

10. Back on the **Overview** page, click on Security

    ![](./media/image38.png)

    >**Note**– As we had enabled **Microsoft Defender for Cloud** earlier, we should be able to see the recommendations for the Server.

11. We should be able to see the recommendations for the on-boarded
    server.

    ![](./media/image39.png)

12. Back on the **Overview** page, click on **Updates**

    ![](./media/image40.png)

13. Click on **Go to Updates using Azure Update Manager** button.

    ![](./media/image41.png)

14. Click on **Check for updates** button.

    ![](./media/image42.png)

15. Click on **OK** button on the **Trigger assess now** message.

    ![](./media/image43.png)

    ![](./media/image44.png)

16. Click on the **Refresh** button, the **Total updates** section will
    show **Assessment is in progress**.

    ![](./media/image45.png)

17. Once the Assessment is completed, click on the Refresh button again.

    ![](./media/image46.png)

18. We should be able to see the details of the required updates on the
    Server.

    ![](./media/image47.png)

19. Click on **One-time update** to start the updates on the Server

    ![](./media/image48.png)

20. On the Install one-time updates page, Machines tab select the
    **WinSrv20224Arc** VM and then click on the **Next** button.

    ![](./media/image49.png)

21. Review the details of the Update and then click on the **Next** button.

    ![](./media/image50.png)

22. On the Properties tab, click on the **Next** button

    ![](./media/image51.png)

23. On the Review + Install tab, review the details and then click on
    **Install** button.

    ![](./media/image52.png)

    ![](./media/image53.png)

24. Click on the Notification to see the update details

    ![](./media/image54.png)

    ![](./media/image55.png)

25. Back on the **Overview** page, click on **Monitoring insights**

    ![](./media/image56.png)

26. Click on the **Analyze data** button

    ![](./media/image57.png)

27. We should now be able to see the **Performance data** of the On-boarded
    server.

    ![](./media/image58.png)

    ![](./media/image59.png)

28. Hence, we have successfully on-boarded the On-premise Windows server
    and we can manage the Server from the Azure Portal Azure Arc.
