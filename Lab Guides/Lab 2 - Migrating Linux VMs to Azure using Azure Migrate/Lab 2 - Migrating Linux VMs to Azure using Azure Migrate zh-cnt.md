# 實驗室 2 - 使用 Azure Migrate 將 Linux 虛擬機器遷移到 Azure

**目標**

在本實驗室中，我們將使用 Azure Migrate - 伺服器遷移工具從內部遷移 Linux
虛擬機工作負載。我們將準備好所需的 Azure
資源，然後在遷移前複製虛擬機器。

**練習 1： Migrate workloads**

**任務 1： Prepare for migration of Hyper-V VMs**

1.  在 **Servers 、 databases and web apps**
    刀片上，通過頁面移動到 **Migration Tools** 部分，在  **Migration and
    modernization** 下選擇 **Discover**。

![A screenshot of a computer Description automatically
generated](./media/image1.png)

2.  在 **Discover** 刀片上，對於下拉式功能表 **Where do you want to
    migrate to?** ，選擇 **Azure Virtual Machines** 選項，在 " **Are
    your Machines virtualized?** 功能表上，選擇 **Yes, with Hyper-V.**

3.  在 **Target region** 功能表上，選擇 **West US 2**。

> ![A close up of a text Description automatically
> generated](./media/image2.png)
>
> **注意** - 確保目的地區域與之前在實驗室 1 中為 **AZMigrateRG**
> 資源組指出的位置/區域**相同**。

4.  選擇**確** **Confirm that the target region for migration is
    eastus** 核取方塊，然後選擇 **Create resources**。

5.  等待遷移項目和 Vault 資源完成部署。

6.  在 **Discover** 刀片上，在 **Prepare Hyper-V host servers** 下，選擇
    **Download** 字樣，*而不是後面的 "**下載 ***"*按鈕。*

> ![](./media/image3.png)
>
> 將下載在 Hyper-V 伺服器上安裝複製提供程式的安裝程式。

7.  在**發現**刀片上，在 **1. Prepare Hyper-V host servers**, ，選擇
    **Download** 按鈕。

> ![A screenshot of a computer Description automatically
> generated](./media/image4.png)
>
> 將下載用於在專案中註冊 Hyper-V 主機的註冊金鑰。

8.  轉到 **Downloads** 檔案夾，然後選擇 **AzureSiteRecoveryProvider**
    檔啟動安裝程式。

9.  在 "Azure Site Recovery Provider 設置（Hyper-V 伺服器）"視窗中，在
    **Microsoft Update** 選項卡上選擇  **On (recommended)** ，然後選擇
    **Next**。

10. 在 **Installation** 選項卡上，接受預設安裝位置，然後選擇
    "**安裝**"。

11. 安裝完成後，選擇 **Register.**

> 如果收到伺服器已註冊的消息，請選擇**重新 Register** 。

12. 在 Azure Site Recovery 嚮導中，在 **Vault Settings**
    選項卡上， **Key file** 右側選擇 **Browse** 。

> ![A screenshot of a computer Description automatically
> generated](./media/image5.png)

13. 轉到 **Downloads** 檔案夾，選擇 **az-migrate-project**
    文件，然後選擇 **Open** 。

> ![A screenshot of a computer Description automatically
> generated](./media/image6.png)
>
> 添加金鑰檔時，將填充金鑰檔、訂閱、Vault 名稱和 Hyper-V 網站名稱值。

14. 選擇 **Next**。

15. 在 **Proxy Settings** 選項卡上，接受預設設置，然後選擇 **Next** 。

> 完成註冊最多需要 5 分鐘。

16. 註冊完成後，選擇 **Finish**。

17. 返回流覽器，在 **Discover** 刀片的 **2. Finalize
    registration**，選擇 **Finalize registration**。

> ![A screenshot of a computer Description automatically
> generated](./media/image7.png)
>
> 您可能需要 **refresh the page** 並重新選擇本任務開始時的選項，以啟用
> "完成註冊 "按鈕。

18. 註冊完成後，您將看到以下資訊。

> ![A close up of a sign Description automatically
> generated](./media/image8.jpeg)
>
> 發現虛擬機器可能需要 15
> 分鐘才能完成，您可能需要刷新頁面才能看到消息。即使任務未完成，也請執行下一項任務。

**任務 2：設置 Azure 資源**

> 既然已經創建了 Azure Migrate 專案，就需要實施目標 Azure 環境。

**Create a Virtual network**

1.  在 Azure Portal 門戶中，在 **Search** 框中輸入虛擬網路，然後選擇
    **Virtual networks**。

2.  在 **Virtual networks**刀片上，選擇 **Create**。

3.  使用以下設置創建虛擬網路。

    - 資源組 - **AZMigrateRG**

    - 虛擬網路名稱 - +++migration-vnet-XXXXXX+++ \[用亂數代替 XXXXXX\]
      虛擬網路 (VNet)

    -  **West US 2** 地區

> **注意** - 確保區域與前面在實驗室 1 中為 **AZMigrateRG**
> 資源組指出的位置/區域**相同**。

**Create a Storage account**

1.  在 Azure Portal 中，在 **Search** 框中輸入 "存儲"，然後選擇
    **Storage accounts**。

2.  在 **Storage accounts** 刀片上，選擇 **Create**。

3.  使用以下設置創建存儲帳戶。所有其他設置保留預設值。

    - 資源組 - AZMigrateRG

    - 存儲帳戶名稱 - +++saXXXXXX+++ \[用亂數代替 XXXXXX］

    - **West US 2** 地區

> **注意** - 確保區域與前面在實驗室 1 中為 **AZMigrateRG**
> 資源組指出的位置/區域**相同**。

- 性能 - **Standard**

- 冗余 - **Locally-redundant storage (LRS)**

4.  在 **Create a storage account** 頁面的
    **Networking** 選項卡上，設置以下設置，並將所有其他設置保留為預設值：

    - 網路訪問 - **Enable public access from selected virtual networks
      and IP addresses**

    - 虛擬網路 (**VNet**)

    - Subnet - **default （10.0.0.0/24）**

> ![A screenshot of a computer Description automatically
> generated](./media/image9.png)

5.  在 **Data protection** 選項卡上，取消選中 **Enable soft delete for
    blobs**。將所有其他設置保留為預設值。

6.  選擇 **Review** ，然後選擇 **Create**。

7.  創建存儲帳戶後，按一下 **Go to Resource**

8.  展開 "資料管理"，選擇 **Data protection** ，然後取消選中 **Enable
    soft delete for blobs**，點擊 **Save** 按鈕。

> ![A screenshot of a computer Description automatically
> generated](./9403877940d3790c97d243e387927393087c7836.png)

**Create a Public IP address**

1.  在 Azure Portal 的 **Search** 框中輸入 "公共 IP"，然後選擇  **Public
    IP addresses**。

2.  在 **Public IP address** 刀片上，選擇 **Create**。

3.  使用以下設置創建公共 IP。

    - 資源組 - **AZMigrateRG**

    - 地區 - **West US 2**

> **注意** - 確保區域與前面在實驗室 1 中為 **AZMigrateRG**
> 資源組指出的位置/區域**相同**。

- 姓名 - +++ipXXXXXX+++ \[用亂數代替 XXXXXX］

- IP 版本 - **IPv4**

- SKU - **Basic**

- IP 位址分配 - **Static**

- 空閒超時（分鐘） - **4**

- DNS 名稱標籤 - +++rhel-web-XXXXXX+++ \[用亂數代替 XXXXXX］

4.  選擇 **Review + create**，然後 **create**

**任務 3：配置 Hyper-V 虛擬機器的複製**

1.  在 Edge 流覽器中打開一個新標籤頁並導航到 URL -
    +++https://portal.azure.com/?feature.customportal=false&feature.canmodifystamps=true&microsoft_azure_migrate=migratecanary#view/Microsoft_Azure_Migrate/AmhResourceMenuBlade/~/getStarted+++

2.  按一下 **Discover、 assess and migrate** 按鈕。

![A screenshot of a computer Description automatically
generated](./media/image11.png)

3.  在  **Migration and modernization** 部分，選擇 **Replicate**。

![A screenshot of a computer Description automatically
generated](./media/image12.png)

4.  您可能需要刷新顯示 **Azure Migrate Servers、 databases and web
    apps** 頁面的流覽器頁面。

5.  在 **Specify intent** 頁面，在  **What do you want to migrate?**
    中選擇 **Servers or virtual machines (VM)**"，在
    "**要遷移到哪裡？"**中選擇 "**Azure VM**

6.  在下拉式功能表 "**您的機器是虛擬化的嗎？**"中選擇 "**是，使用
    Hyper-V**"，然後按一下 "**繼續** "按鈕。

7.  在複製頁面的**虛擬機器**選項卡上，使用以下設置完成複製標準。

    - 從 Azure Migrate 評估導入遷移設置 - **是，應用 Azure Migrate
      評估中的遷移設置**

    - 選擇組 - **RHEL-Servers**

    - 選擇評估 - **AS-43240741**

    - 虛擬機器 **RHEL-DB-01** 和 **RHEL-WEB-01**

8.  在複製頁面的**目標設置**選項卡上，使用以下設置指定目標詳細資訊。

    - 資源組 - **AZMigrateRG**

    - 緩存存儲帳戶 - **saXXXXXX**

    - 虛擬網路 (**VNet)** - **migration-vnet-XXXXXX**

    - Subnet - **預設值**

9.  在複製頁面的**計算**選項卡上，在兩個虛擬機器上使用以下設置：

    - Azure 虛擬機器大小 - **Standard_D2s_v3**

    - 作業系統類型 - **Linux**

10. 將其餘選項卡中的設置保留為預設值，然後選擇**複製**。

11. 返回 "**Azure 遷移伺服器、資料庫和 Web 應用程式** "頁面，選擇
    "**刷新**"，然後在 "**遷移和現代化** "部分，選擇 "**概述**"。

![A screenshot of a computer Description automatically
generated](./media/image13.png)

12. 在 "遷移和現代化 "頁面的 "**複製** "部分，檢查複製機器清單中的
    "**狀態** "列。

![A screenshot of a computer Description automatically
generated](./media/image14.png)

等候狀態變為 "**受保護"**。這可能需要額外 15 分鐘。

您需要刷新 "**遷移和現代化 複製機器** "以更新狀態資訊。

**任務 4：執行測試遷移**

1.  在 Azure Portal 的 "**遷移和現代化 | 複製** "頁面中，選擇
    **RHEL-DB-01** 虛擬機器。

![A screenshot of a computer Description automatically
generated](./media/image15.png)

2.  在 **RHEL-DB-01** 頁面，選擇**測** **Test migration**。

![](./media/image16.png)

3.  選擇 **migration-vnet-XXXXXX** 虛擬網路，然後選擇**測 Test
    migration**。

4.  返回  **Migration and modernization Replicating machines**
    頁面，然後選擇 **RHEL-WEB-01** 虛擬機器。

5.  在 **RHEL-WEB-01** 頁面，使用 **migration-vnet-XXXXXX**
    虛擬網路啟動**測試遷移**。

6.  返回 **Migration and modernization Replicating machines**頁面。
    **Replication status** 應為 **Initiating test failover**

![](./media/image17.png)

等待**測試容錯移轉**完成。這可能需要 5-7 分鐘。

7.  返回 **Migration and modernization Replications** 頁面，選擇
    **Refresh** ，然後驗證兩台虛擬機器是否都處於 " **Cleanup test
    failover pending** 狀態。

**Validate test migrations**

8.  在 Azure Portal
    中，在**搜索**框中輸入虛擬機器，然後選擇**虛擬機器**。

9.  注意代表新複製的虛擬機器的條目。

注意：最初，虛擬機器的名稱將由 **asr-temp**
首碼和隨機生成的尾碼組成，但會自動重命名為 **RHEL-DB-01-test** 和
**RHEL-WEB-01-test**。

10. 在**虛擬機器**頁面，選擇 **RHEL-WEB-01-test** 虛擬機器。

![A screenshot of a computer Description automatically
generated](./media/image18.png)

11. 在 **RHEL-WEB-01-test** 頁面的 **Settings**  下選擇 **Networking**。

12. 在 **Networking** 刀片上，選擇網路介面 **nic-RHEL-WEB-01-00-test**

![A screenshot of a computer Description automatically
generated](./media/image19.png)

13. 在 **nic-RHEL-WEB-01-00-test** 頁面，在 **"設置 "**下選擇 **"IP
    配置"**。

14. 選擇 **nic-RHEL-WEB-01-00-test-ipConfig** 編輯 IP **配置**。

![A screenshot of a computer Description automatically
generated](./media/image20.png)

15. 在**編輯 IP 配置**刀片中，選中**關聯公共 IP
    位址核取方塊**，然後為 **Public IP address** 選擇 **ip43240741**。

16. 選擇**保存**，等待關聯完成。

17. 打開新的 Edge 標籤，轉到為公共 IP 分配的 **DNS name** ：

> +++rhel-web-XXXXXX.westus2.cloudapp.azure.com+++++

18. 驗證 RHEL-WEB-01-test 上託管的 Drupal 網站是否載入成功。

19. 如果網站無法打開，請在虛擬機器的**網路設置**中創建一個**網路安全性群組**，並啟用**埠
    80**，如下圖所示。

![](./media/image21.png)

![A screenshot of a computer Description automatically
generated](./b45e02540b0d40b862b56a1ef71fc116015ae4e1.png)

**Cleanup test migrations**

19. 返回 "**遷移和現代化 | 複製** "頁面，選擇 **RHEL-DB-01**。

20. 選擇 **Clean up test migration** 操作。

![](./media/image23.png)

21. 將**備註**欄位留空，並選擇**測試已完成**核取方塊**。 Delete test
    virtual machine**，然後選擇 **Cleanup Test**

22. 返回 **Migration and modernization | Replications** 頁面，選擇
    **RHEL-WEB-01**。

23. 選擇 **Clean up test migration**，指定 **Testing is complete。
    Delete test virtual machine**。

24. 返回 **Migration and modernization | Replications** 頁面。

25. 等待 **Replication status 態**為 **Protected** 後再繼續。

您可能需要在一兩分鐘後選擇 "**刷新** "才能看到更新。

**任務 5：執行遷移**

1.  選擇 **RHEL-DB-01** 並觸發**遷移**操作。

![A screenshot of a computer Description automatically
generated](./media/image24.png)

2.  在 **"遷移** "頁面上，確保
    "**遷移前關閉機器以儘量減少資料丟失？**"選項設置為
    **"是"**，然後選擇 "**遷移"**。

3.  返回 "**遷移和現代化 | 複製** "頁面，選擇 **RHEL-WEB-01**。

4.  選擇 "**遷移** "並開始遷移，再次在 "遷**移** "頁面中指定 "**是**"。

5.  返回 "**遷移和現代化 | Replications** "頁面，選擇 "**刷新**
    "以監控遷移狀態。

![](./media/image25.png)

6.  讓 Edge 處於打開狀態，以便進行下一步操作。遷移將繼續處理。

**練習 2：遷移後的任務**

**任務 1：完成遷移後任務**

在本練習中，您將為新遷移的 RHEL-WEB-01 虛擬機器分配之前創建的公共 IP。

1.  在 "**遷移和現代化 | 複製** "頁面上，驗證 "**狀態**
    "列是否顯示兩個虛擬機器**的計畫故障切換均已完成**。

您可能需要選擇 "**刷新** "才能看到此更新。

2.  在 Azure Portal
    中，在**搜索**框中輸入虛擬機器，然後選擇**虛擬機器**。

3.  在**虛擬機器**頁面，選擇 **RHEL-WEB-01** 虛擬機器。

4.  在 **RHEL-WEB-01** 頁面的 "**設置 "**下選擇 "**網路**"。

5.  在**網路**刀片上，選擇網路介面 **nic-RHEL-WEB-01-00**

![](./media/image26.png)

6.  在 **nic-RHEL-WEB-01-00** 頁面的 "**設置 "**下選擇 **"IP 配置"**。

7.  選擇 **nic-RHEL-WEB-01-00-ipConfig** 編輯 IP **配置**。

![A screenshot of a computer Description automatically
generated](./media/image27.png)

8.  在**編輯 IP 配置**刀片中，選中**關聯公共 IP
    位址核取方塊**，然後為**公共 IP 位址**選擇 **ip43240741**。

9.  選擇**保存**，等待關聯完成。

10. 打開新的 Edge 標籤，轉到為公共 IP 分配的 **DNS 名稱**：

> +++rhel-web-XXXXXX.westus2.cloudapp.azure.com+++++

11. 如果網站無法打開，請在虛擬機器的**網路設置**中創建**網路安全**性群組並啟用**埠
    80。**

12. 驗證 RHEL-WEB-01 上託管的 Drupal 網站是否已載入。

13. 打開 **Hyper-V
    管理器**，注意兩個虛擬機器都已**關閉**。這些機器已成功遷移。

![A screenshot of a computer Description automatically
generated](./media/image28.png)
