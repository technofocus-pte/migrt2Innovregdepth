# ラボ5 - Defender for Cloud & Sentinel。

**タスク 1：Microsoft Defender for CloudからVMsのJITを有効にします。**

1.  **Azure Portal** ```https://portal.azure.com``` で検索ボックスに
    ```Microsoft Defender for Cloud``` と入力し、**Services** の下の
    **Microsoft Defender for Cloud** をクリックします。

    ![](./media/image1.png)

2.  **Microsoft Defender for
    Cloud｜Overview」** ページの左側ペインで、**「Cloud
    Security」** セクションに移動し、「**Workload
    Protections**」をクリックします。

    ![](./media/image2.png)

3.  **Microsoft Defender for Cloud｜Workload
    protections** ページで、下図に示すように、下にスクロールし、**「Advanced
    protection** **」** セクションの「**Just-in-time VM
    access**」をクリックします。

    ![](./media/image3.png)

4.  **Just-in-time VM access** ページで、**Virtual
    machines**セクションに移動し、**Non
    Configured**タブをクリックします。VMs - PostgreSrvが**Non
    Configured**タブの下に表示されます。

    ![](./media/image4.png)

5.  利用可能なVMを1つ選択し、右側の**Enable JIT on 1
    VM**ボタンをクリックする。

    ![](./media/image5.png)

6.  **JIT VM access configuration** ページで、**Saveを**クリックする。

    ![](./media/image6.png)

7.  **Just-in-time VM access configuration has
    startedという通知が届きます**。

    ![](./media/image7.png)

8.  **Virtual
    machines**セクションの**Configured**タブをクリックすると、VMsのPostgreSrvが**Configured**タブにリストされていることがわかります。

    ![](./media/image8.png)

9.  このVMに接続するためには、リクエストに応じてアクセスが許可される。

    ![](./media/image9.png)

**タスク 2: セキュリティ警告の生成と調査**

1.  **Microsoft Defender for
    Cloudの**「**General** **」** セクションで「**Security
    alerts**」を選択します。　　

    ![](./media/image10.png)

2.  **Sample alerts** ボタンをクリックしてアラートを生成します。

    ![](./media/image11.png)

3.  **Create sample alerts**ボタンをクリックしてください。

    ![](./media/image12.png)

    ![](./media/image13.png)

4.  サンプルアラートが生成されます

    ![](./media/image14.png)

5.  **Refresh**ボタンをクリックすると、サンプルアラートが表示されます。

    ![](./media/image15.png)

6.  調査したいアラートをクリックしてください。

7.  アラート概要ペインで、以下の詳細を調査する。

    1.  **重症度、状態、活動時間**

    2.  検出された正確な活動を**説明**する**記述**

    3.  **影響を受ける資源**

    4.  MITRE ATT&CK マトリックス上の活動の**キルチェーンインテント**。

8.  不審な行動を調査するのに役立つ詳細情報については、「**View full
    details** **」** ボタンをクリックしてください。

    ![](./media/image16.png)

9.  **Alert details** タブで情報を確認する。

    ![](./media/image17.png)

10. **Take
    action**ボタンをクリックし、利用可能なオプションを確認します。

    ![](./media/image18.png)

**エクササイズ2 - Sentinelの展開**

**タスク 1：Microsoft Sentinelワークスペース**

この実習では、Microsoft
Sentinelワークスペースを作成する方法を見ていきます。

1.  ```https://portal.azure.com```に移動し、ラボ環境のLabリソースで提供される**MOD
    Administrator** 資格情報でログインします。　

2.  上の検索バーに「```Microsoft Sentinel```」と入力し、「**Microsoft
    Sentinel**」をクリックします。

    ![](./media/image19.png)

3.  **Microsoft Sentinel**画面で、左上の**Create**をクリックします。

    ![](./media/image20.png)

4.  **Microsoft Sentinelを**既存の**Log
    Analyticsワークスペースに**追加するか、新しいワークスペースを構築するかを選択できます。ここでは新しいものを作成するので、**Create
    a new workspaceを**クリックする。

    ![](./media/image21.png)

5.  **Create Log
    Analyticsワークスペースの**ページで、以下のように入力します：

    1.  サブスクリプション**Azure Pass - Sponsorship**

    2.  リソースグループ：**Create new** 
        ```LAWResourceGroup```をクリックする。

    3.  ワークスペース名```SentWrkspcXXXXXX```
        \[XXXXXXを乱数に置き換えてください。］

    4.  地域: **West US**

    5.  **Review + create**をクリックする。

    ![](./media/image22.png)

6.  検証が完了したら、「**Create**」をクリックします。作成には数秒かかります。

    ![](./media/image23.png)

7.  **Add Microsoft**
    Sentinelをワークスペースページに戻るので、**Refresh**ボタンをクリックします。

    ![](./media/image24.png)

8.  先ほど作成したワークスペースを選択し、下部にある「**Add**」をクリックします。

    ![](./media/image25.png)

9.  以下の画像のような通知が届くはずです。

    ![](./media/image26.png)

10. Microsoft
    Sentinelワークスペースを使用する準備ができましたら、OKボタンをクリックしてください。

    ![](./media/image27.png)

**タスク 2: データコネクタを有効にする。**

この演習では、データコネクタを有効にする方法を説明します。

1.  ブラウザのタブで
    ```https://portal.azure.com/#view/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/AllUsers```
    に移動し、**Tenant Administrator accountを**選択します。　

    ![](./media/image28.png)

2.  「管理」で**Assigned roles** **を**選択し、**+ Add
    assignments** をクリック **します。**

    ![](./media/image29.png)

3.  検索して **「Security
    Administrator」** を選択し、**「Add」** ボタンをクリックする。

    ![](./media/image30.png)

    ![ ](./media/image31.png)

4.  Azure Portal ```https://portal.azure.com``` で Microsoft Sentinel
    を検索し、**Microsoft Sentinel** をクリックする。

    ![](./media/image32.png)

5.  SentWrkspcXXXXXXを選択する。

    ![](./media/image33.png)

6.  次に、**Configuration**セクションで**Data Connectorsを**選択する。

    ![](./media/image34.png)

7.  **Data Connector with "content source = gallery content" have been
    removedという **メッセージが表示されるはずです**。** そのメッセージで、**Click
    here** リンクを選択します。　

    ![](./media/image35.png)

8.  **Out-of-the-box Content
    Centralization** ページで「**Continue**」をクリックします。　

    ![](./media/image36.png)

9.  **Complete centralization** ボタンをクリック　

    ![](./media/image37.png)

      <font color=darkgreen>
      
    > **注** -
    コネクタがインストールされない場合がありますが、ラボを進めるには問題ありません。

    </font>

10. 以下の画像のような通知が届くはずです。

    ![A close-up of a sign ](./media/image38.png)

11. 一番上から**Microsoft
    Sentinelの**リンクをクリックするか、Sentinelページに戻ってください。

    ![A screen shot of a computer ](./media/image39.png)

12. **Refresh**ボタンをクリックすると、Data
    connectorsがいくつか表示されるはずです。

    ![](./media/image40.png)


13. **Content managementのContent hub** **を**クリックします。

    ![](./media/image41.png)

14. Content hubページでAzure Activityを検索し、**Azure
    Activity**コンテンツを選択して**Install**ボタンをクリックします。

    ![](./media/image42.png)

15. コンテンツハブのページでMicrosoft Defender for
    Cloudを検索し、**Microsoft Defender for
    Cloudの**コンテンツを選択し、**Install**ボタンをクリックします。

    ![](./media/image43.png)

**タスク3：Azure Activity data connectorを有効にする**

この演習では、Azure
Activityデータコネクタを有効にする方法を示します。このコネクタは、Azureサブスクリプションで実行されたアクションのすべての監査イベントをMicrosoft
Sentinelワークスペースにもたらします。

1.  **Microsoft Sentinel**ページで、**Configuration**セクションの**Data
    Connectorsを**クリックします。

    ![](./media/image44.png)

2.  Data Connectors 画面で、検索バーに Activity と入力し、**Azure
    Activity** コネクターを選択して、**Open connector page**
    をクリックします。

    ![](./media/image45.png)

3.  **Azure Activity**コネクタページで、オプション番号**2. Connect your
    subscriptions through diagnostic settings new
    pipeline**に進みます **。** この方法では、Azure Policy
    を活用し、従来の方法と比較して多くの改善がもたらされます（これらの改善の詳細については、こちらを参照してください）。**Launch
    Azure Policy
    Assignment**ウィザードをクリックすると、ポリシー作成ページにリダイレクトされます。

    ![](./media/image46.png)

4.  **Scopesの**選択で **Azure Pass - Sponsorship**
    を選択します。**Selectを**クリックします。

    ![Screens screenshot of a computer ](./media/image47.png)

5.  **\[Parameters］**タブに移動します。**Primary Log Analytics
    workspace** で**MicrosoftSentinelWorkspaceを**選択します。

    ![](./media/image48.png)

6.  **Remediation**タブで、**\[Create a remediation
    task** **\]**以外のチェックボックスを選択し、**\[Review +
    create** **\]** ボタンをクリックします。

    ![ computer screen ](./media/image49.png)

7.  **Review + create**タブで、**Create**ボタンをクリックする。

    ![](./media/image50.png)

8.  **Notification**ペインでは、「‘**Role Assignments creation
    succeeded**’, ‘**Remediation task creation succeeded**’ と
    ‘**Creating policy assignment
    succeeded**’の通知を見ることができます。

    ![](./media/image51.png)

9.  **Azure Activity
    connectorの**ページで、接続ステータスを確認できます。

    ![](./media/image52.png)

    > **注**：すぐにコネクターが**Connected**と緑色で表示されなくても、プロセスが完了するまで約30分かかります。

10. 次のエクササイズに進み、30分後にまたチェックできる。

**タスク 4: Microsoft Defender for Cloud
データコネクタを有効にします。**

この演習では、Microsoft Defender for Cloud data
connectorを有効にする方法を説明します。このコネクタを使用すると、Microsoft
Defender for CloudからMicrosoft
Sentinelにセキュリティアラートをストリームできるため、ワークブックでDefenderデータを表示したり、アラートを生成するためにクエリを実行したり、インシデントを調査して対応したりできます。

1.  **Microsoft Sentinel**ページで、**Configuration**セクションの**Data
    Connectorsを**クリックします。

    ![](./media/image44.png)

2.  **Data connectors**画面で、検索バーにtenantと入力し、**Tenant-based
    Microsoft Defender for Cloud (Preview)** コネクタを選択し、**Open
    connector pageを**クリックします。

    ![](./media/image53.png)

    <font color=red>

    > **注意** - **Data Connector Not
    Foundという**エラーが表示された場合は、**Content
    Hubに**移動し、**Microsoft Defender for Cloud
    Connectorを**再インストールしてください。

    </font>

    ![A black text on a white background ](./media/image54.png)

    ![](./media/image55.png)

3.  **テナントベースの Microsoft Defender for Cloud (Preview)**
    コネクタページで、**Configuration** セクションの **Connect**
    ボタンをクリックします。

    ![](./media/image56.png)

4.  **Connected
    successfully（接続に成功した）**の通知が届くはずです**。**

    ![](./media/image57.png)

5.  1～2分待ってからページを更新すると、コネクタのステータスも**Connectedに**更新されているはずです **。**

    ![](./media/image58.png)

6.  **Data
    connectors**画面に戻り、SearchバーにSubscriptionと入力し、**Subscription-based
    Microsoft Defender for Cloud (legacy)**コネクタを選択し、** Open
    connector pageを**クリックします。

    ![](./media/image59.png)

7.  **Subscription-based Microsoft Defender for
    Cloud** **(legacy)** コネクタページの **\[Configuration\]**
    セクションで、**Azure Pass - Sponsorship**
    サブスクリプションを選択し、**\[Connect\]**
    ボタンをクリックします。　　

    ![](./media/image60.png)

8.  **Connected successfully**の通知が届くはずです。　

    ![](./media/image61.png)

9.  コネクタの Status も **Connected** に更新する **。**

    ![](./media/image62.png)

**練習3-統合**

Defender for Cloudコネクタをインストールしたので、Sample
Alertsを使用して生成されたMicrosoft Defender for
CloudからのIncidentを見ることができるはずです。

1.  **Microsoft
    Sentinel**ページで、脅威管理の下の**Incidentsを**クリックします。　

    ![](./media/image63.png)

2.  **Microsoft Defender for
    Cloud**コネクタを有効にしたばかりなので、インシデントが表示されるまで約20～30分かかります。

3.  **「General**」の「**Overview**」をクリックし、**「New
    overview** **」** スイッチを「**Off**」に切り替えます。　

    ![](./media/image64.png)

4.  スイッチがオフになると、Microsoft Defender for Cloudからの**Sample
    eventsを**見ることができるはずです。

    ![](./media/image65.png)

5.  **SecurityAlertsを**クリックします。

    ![](./media/image66.png)

6.  Log Analyticワークスペースが開き、**Microsoft Defender for
    Cloudから**生成および同期された**Alertsの**すべてのログが一覧表示されます。

    ![](./media/image67.png)

7.  **Alertsを**クリックすると、アラートの詳細が表示されます。

    ![](./media/image68.png)

8.  アラートの詳細が拡大されている。　

    -  TimeGenerated \[UTC］

    -  表示名

    -  アラート名

    -  アラート重大度

    -  検出された正確な活動を説明する記述

    -  ProviderName - Azure Security Center (Microsoft Defender for Cloud の旧名称。)

    -  リメディタステップ

    -  その他の行にも追加情報がある。

    ![ computer error ](./media/image69.png)
