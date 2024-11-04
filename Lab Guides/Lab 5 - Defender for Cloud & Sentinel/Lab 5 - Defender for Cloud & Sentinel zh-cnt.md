# 实验室 5 - 云和 Sentinel 的 Defender。

**任务 1：从 Microsoft Defender for Cloud 在虚拟机上启用 JIT**

1.  在 **Azure Portal** +++https://portal.azure.com+++ 的搜索框中输入
    +++Microsoft Defender for Cloud+++，然后单击**服务**下的 **Microsoft
    Defender for Cloud**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  在 **Microsoft Defender for Cloud | Overview** 
    页面左侧窗格中，导航到 **Cloud Security** 部分，然后单击 **Workload
    Protections** 。

![](./media/image2.png)

3.  在 **Microsoft Defender for Cloud | Workload
    protections** 页面，向下滚动并单击 **Advanced protection**
    部分下的 **Just-in-time VM access** ，如下图所示。

![A screenshot of a computer Description automatically
generated](./media/image3.png)

4.  在 **"Just-in-time VM access（即时虚拟机访问**）"页面上，导航到
    "**Virtual machines**（**虚拟机）**"部分，点击 "**Non
    Configured（未配置**）"选项卡。你会看到虚拟机 - **PostgreSrv** 列在
     **Non** **Configured** 选项卡下。

![](./media/image4.png)

5.  选择一个可用虚拟机，然后单击右侧的 **Enable JIT on 1 VM** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image5.png)

6.  在 **JIT VM access configuration** 页面，单击 **Save**。

![A screenshot of a computer Description automatically
generated](./media/image6.png)

7.  您将收到一条通知 - **Just-in-time VM access configuration has
    started**。

![](./media/image7.png)

8.  单击 "**虚拟机** "部分下的 **Configured**  选项卡，你会看到虚拟机
    **PostgreSrv** 列在 **Configured**  选项卡下。

![A screenshot of a computer Description automatically
generated](./media/image8.png)

9.  现在，要连接到该虚拟机，就必须根据请求授予访问权限。

![](./media/image9.png)

**任务 2：生成和调查安全警报**

1.  在 **Microsoft Defender for Cloud 的 General** 部分下选择 **Security
    alerts**

![](./media/image10.png)

2.  单击  **Sample alerts** 按钮生成警报。

![](./media/image11.png)

3.  单击  **Create sample alerts** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

![](./media/image13.png)

4.  将生成警报样本

![A screenshot of a computer Description automatically
generated](./media/image14.png)

5.  点击 **Refresh**  按钮，您就可以看到示例警报。

![](./media/image15.png)

6.  您可以点击任何想要调查的警报

7.  在警报概览窗格中，调查以下详细信息。

    1.  **Severity, Status, and Activity time**

    2.  **Description** 被检测到的准确活动的**描述**

    3.  **Affected resources**

    4.  MITRE ATT&CK 矩阵上活动的 **Kill chain intent** 。

8.  如需有助于调查可疑活动的更详细信息，请单击 "**查看全部详情** "按钮。

![](./media/image16.png)

9.  查看 **Alert details** 选项卡下的信息。

![](./media/image17.png)

10. 点击 **Take action** 按钮，查看可用选项

![](./media/image18.png)

**练习 2 - 部署 Sentinel**

**任务 1：Microsoft Sentinel 工作区**

在本练习中，我们将了解如何创建 Microsoft Sentinel 工作区。

1.  导航至 +++https://portal.azure.com+++
    并使用实验室环境的实验室资源提供的 **MOD Administrator** 凭据登录。

2.  在顶部搜索栏中输入 +++Microsoft Sentinel+++，然后点击 **Microsoft
    Sentinel**。

![](./media/image19.png)

3.  在 **Microsoft Sentinel** 屏幕中，单击左上角的 **Create** 。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

4.  您可以选择将 **Microsoft Sentinel** 添加到现有的 **Log
    Analytics** **workspace**
    或创建一个新的工作区。我们将创建一个新的**工作区**，请单击 **Create
    a new workspace** 。

![A screenshot of a computer Description automatically
generated](./media/image21.png)

5.  在 **Create Log Analytics workspace** 页面，填写如下表格：

    1.  订阅： **Azure Pass - Sponsorship**

    2.  资源组： **Create new** +++LAWResourceGroup++++

    3.  工作区名称：+++SentWrkspcXXXXXX+++\[用随机数代替 **XXXXXX］**

    4.  地区：  **West US**

    5.  单击 **Review + create**。

![](./media/image22.png)

6.  验证完成后点击 **Create** . 创建过程需要几秒钟。

![](./media/image23.png)

7.  您将被重新定向到 **Add Microsoft Sentinel** 到工作区 "页面，单击
    **Refresh**  按钮。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

8.  选择刚刚创建的工作区，然后单击底部的 **Add** 。

![A screenshot of a computer Description automatically
generated](./media/image25.png)

9.  您应该会收到如下图所示的通知

![A screenshot of a computer Description automatically
generated](./media/image26.png)

10. 您的 Microsoft Sentinel 工作区已准备就绪，请单击 "**确定**
    "按钮继续。

![](./media/image27.png)

**任务 2：启用数据连接器。**

本练习演示如何启用数据连接器。

1.  在浏览器标签页导航至
    +++https://portal.azure.com/#view/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/AllUsers+++
    并选择 **Tenant Administrator account**

![A screenshot of a computer Description automatically
generated](./media/image28.png)

2.  选择 "管理 "下的 **Assigned roles** ，然后点击 **+ Add assignments
    。**

![](./media/image29.png)

3.  搜索并选择 **Security Administrator**，然后单击 **Add** 按钮。

![A screenshot of a computer Description automatically
generated](./media/image30.png)

![A close-up of a black text Description automatically
generated](./media/image31.png)

4.  在 Azure Portal https://portal.azure.com 并搜索 Microsoft
    Sentinel，然后单击 Microsoft Sentinel。

![A screenshot of a computer Description automatically
generated](./media/image32.png)

5.  选择 **SentWrkspcXXXXXX**。

![A screenshot of a computer Description automatically
generated](./media/image33.png)

6.  现在选择 **Configuration**  部分下的 **Data Connectors**。

![A screenshot of a computer Description automatically
generated](./media/image34.png)

7.  您应该会收到 **Data Connector with "content source = gallery
    content" have been removed 的**消息**。**在该信息中选择 **Click
    here** 链接

![A screenshot of a computer Description automatically
generated](./media/image35.png)

8.  在 **Out-of-the-box Content Centralization** 页面点击 **Continue**

![A screenshot of a computer Description automatically
generated](./media/image36.png)

9.  单击  **Complete centralization** 按钮

![A screenshot of a computer Description automatically
generated](./media/image37.png)

10. 您应该会收到如下图所示的通知

![A close-up of a sign Description automatically
generated](./media/image38.png)

11. 从顶部单击 **Microsoft Sentinel** 链接或返回 Sentinel 页面。

![A screen shot of a computer Description automatically
generated](./media/image39.png)

12. 点击 **Refresh**  按钮，你应该可以看到显示的几个连接器数据连接器。

![](./media/image40.png)

**注意** - 有时可能无法安装任何连接器，但也没关系，可以继续进行实验室。

13. 点击 **Content hub** 下的 **Content management**

![A screenshot of a computer Description automatically
generated](./media/image41.png)

14. 在内容中心页面搜索 Azure Activity，然后选择 **Azure Activity**
    内容并单击 **Install** 按钮

![A screenshot of a computer Description automatically
generated](./media/image42.png)

15. 在内容中心页面搜索 Microsoft Defender for Cloud，然后选择
    **Microsoft Defender for Cloud** 内容，点击 **Install** 按钮

![](./media/image43.png)

**任务 3：启用 Azure 活动数据连接器**

本练习将向您演示如何启用 Azure 活动数据连接器。此连接器将把在您的 Azure
订阅中执行的操作的所有审计事件带入 Microsoft Sentinel 工作区。

1.  在 **Microsoft Sentinel** 页面上点击 **Configuration** 部分下的
    "**数据连接器**"。

![A screenshot of a computer Description automatically
generated](./media/image44.png)

2.  在数据连接器界面，在搜索栏中输入 activity，选择 **Azure Activity
    连接器**，然后点击 **Open connector page** 。

![A screenshot of a computer Description automatically
generated](./media/image45.png)

3.  在 **Azure 活动连接器**页面，转到选项
    **2。通过诊断设置新管道连接订阅**。这种方法利用了 Azure
    Policy，与旧方法相比，它带来了许多改进（有关这些改进的更多详细信息，请点击此处）。单击**启动
    Azure Policy Assignment** 向导，这会将你重定向到策略创建页面。

![A screenshot of a computer Description automatically
generated](./media/image46.png)

4.  在 "**范围** "选项中选择 "**Azure Pass -– Sponsorship** "。单击
    **Select** 。

![Screens screenshot of a computer Description automatically
generated](./media/image47.png)

5.  转到 **Parameters** 选项卡。在 **Primary Log Analytics workspace
    中**选择 **MicrosoftSentinelWorkspace**。

![A screenshot of a computer Description automatically
generated](./media/image48.png)

6.  在 **Remediation**  选项卡下，选择 **Create a remediation task**
    以外的复选框，然后单击 **Review + create** 按钮

![A screenshot of a computer screen Description automatically
generated](./media/image49.png)

7.  在  **Review + create** 选项卡上，单击 **Create**  按钮。

![A screenshot of a computer Description automatically
generated](./media/image50.png)

8.  在 **Notification**  窗格中，你可以看到 **Role Assignments creation
    succeeded**、 ‘**Remediation task creation succeeded**’ 和
    **Creating policy assignment succeeded**’通知。

![A screenshot of a computer Description automatically
generated](./media/image51.png)

9.  在 **Azure Activity connector** 页面上，你将能看到连接状态。

![A screenshot of a computer Description automatically
generated](./media/image52.png)

**注意**：如果您没有立即看到连接器显示为 **Connected**  和
"绿色"，这是正常现象，整个过程大约需要 30 分钟。

10. 继续下一个练习，然后您可以在 30 分钟后再回来检查。

**任务 4：为云数据连接器启用 Microsoft Defender。**

本练习向您演示如何启用 Microsoft Defender for Cloud
数据连接器。此连接器允许您将 Microsoft Security 警报从 Microsoft
Defender for Cloud 流式传输到 Microsoft
Sentinel，这样您就可以在工作簿中查看 Defender
数据、查询数据以生成警报，以及调查和响应事件。

1.  在 **Microsoft Sentinel** 页面上点击 **Configuration**  部分下的
    **Data Connectors** 。

![](./media/image44.png)

2.  在  **Data
    connectors** 页面中，在搜索栏中输入**租户**，选择**基于租户的
    Microsoft Defender for Cloud（预览版）**连接器，然后点击 **Open
    connector page** 。

![A screenshot of a computer Description automatically
generated](./media/image53.png)

**注意** - 如果收到 **Data Connector Not Found**，请导航至 **Content
Hub**，然后再次重新安装 **Microsoft Defender for Cloud Connector**。

![A black text on a white background Description automatically
generated](./media/image54.png)

![A screenshot of a computer Description automatically
generated](./media/image55.png)

3.  在**基于租户的 Microsoft Defender for
    Cloud（预览版）**连接器页面上，在 "**配置** "部分下单击 "**连接**
    "按钮。

![](./media/image56.png)

4.  您应该会收到 **Connected successfully 。**

![A screenshot of a computer Description automatically
generated](./media/image57.png)

5.  等待 1-2 分钟，然后刷新页面，连接器的状态也应更新为 **Connected 。**

![](./media/image58.png)

6.  回到**数据连接器**页面，在搜索栏中输入订阅，选择**基于订阅的
    Microsoft Defender for Cloud（传统）**连接器，然后单击 **Open
    connector page**。

![](./media/image59.png)

7.  在**基于订阅的 Microsoft Defender for
    Cloud（传统）**连接器页面上，在**配置**部分下，选择 **Azure 通行证 -
    赞助**订阅，然后单击 **Connect**  按钮。

![A screenshot of a computer Description automatically
generated](./media/image60.png)

8.  您应该会收到 C**onnected successfully 的**通知。

![](./media/image61.png)

9.  连接器的状态也应更新为 **Connected 。**

![](./media/image62.png)

**练习 3 - 整合**

由于我们已经安装了 Defend for Cloud
连接器，因此应该可以看到使用示例警报从 Microsoft Defender for Cloud
生成的事件。

1.  在 **Microsoft Sentinel** 页面上单击 "威胁管理 "下的
    **Incidents** 。

![A screenshot of a computer Description automatically
generated](./media/image63.png)

2.  由于我们刚刚启用了 **Microsoft Defender for Cloud**
    连接器，事件出现大约需要 20-30 分钟。

3.  单击 **General** 下的 **Overview** ，然后将 **New
    overview** 开关切换为 **Off**

![](./media/image64.png)

4.  关闭开关后，我们应该就能看到 Microsoft Defender for Cloud
    的**示例事件**了。

![](./media/image65.png)

5.  单击 **SecurityAlerts**

![A screenshot of a computer Description automatically
generated](./media/image66.png)

6.  它应该会打开日志分析工作区，并列出 **Microsoft Defender for Cloud**
    生成和同步的所有**警报**日志。

![](./media/image67.png)

7.  点击任何 **Alerts** , 即可展开并列出相关详细信息。

![A screenshot of a computer Description automatically
generated](./media/image68.png)

8.  在警报的扩展详情中，您可以看到

    1.  时间生成 \[UTC］

    2.  显示名称

    3.  警报名称

    4.  警报严重程度

    5.  说明被检测到的准确活动的描述

    6.  ProviderName - Azure Security Center - Microsoft Defender for
        Cloud 的旧名称

    7.  补救步骤

    8.  以及其他行的附加信息。

![A screenshot of a computer error Description automatically
generated](./media/image69.png)
