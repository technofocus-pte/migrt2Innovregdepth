# ラボ4 - アダプティブクラウド

**目的**

このラボでは、Azure Arc
を使用して環境全体でリソースを管理します。オンプレミス、他のパブリック
クラウド、エッジ
デバイスなど、環境全体でインフラストラクチャを保護、監視、管理できます。

**タスク1 - オンプレミス・マシンのセットアップ**

1.  Lab VM 上で Edge ブラウザを開き、**AzCopy**
    ファイルをダウンロードするリンクに移動します -
    ```https://aka.ms/downloadazcopy-v10-windows``` zip
    ファイルを開き、フォルダ ```C:\AzCopy``` に解凍します。

    ![ a computer ](./media/image1.png)

2.  Star-menを右クリックし、Windows PowerShell（管理者）を選択します。

3.  **Windows Server
    2022イメージを**ダウンロードするには、以下のコマンドを入力します。

    ```cmd```

    ```cdAzCopy```


    ```cd``` を押してから **Tab キー**を押してフォルダ名を自動入力し、Enter を押します。


    ```azcopy copy "https://strg4vmimages2024.blob.core.windows.net/images/WinSrv20224Arc.zip" "C:\Users\Administrator\Downloads"```


    <font color=Darkgreen>

    > 上記のコマンドを実行すると、**Windows Server
    2022の**イメージがダウンロード・フォルダーにコピーされます。イメージのダウンロードには最大7～10分かかります。

    </font>

    ![ a computer screen ](./media/image2.png)     ![ a computer Description
automatically generated](./media/image3.png)

4.  ダウンロードが完了したら、ファイルエクスプローラーでダウンロードフォルダを開き、**WinSrv20224Arc.zipを**選択し、「Extract
    all」ボタンをクリックします。

    ![ a computer ](./media/image4.png)

5.  ```E:\Virtual Machines```とフォルダを指定し、Extract ボタンをクリックします。

    ![ a computer ](./media/image5.png)

6.  タスクバーから**Hyper-V
    Managerを**開き、サーバー名を右クリックして**Hyper-V
    Settingを**選択する。


    ![ a computer ](./media/image6.png)

7.  **「Settings」**ウィンドウで、**「Enhanced Session Mode
    Policy**」オプションを選択し、「**Allow enhanced session
    mode 」のチェックボックスを有効にして、**「OK」ボタンをクリックします。　　

    ![](./media/image7.png)

8.  **Hyper-V ManagerでImport Virtual
    Machine**アクションをクリックします。

    ![ a computer ](./media/image8.png)

9.  **Locate
    Folder**（フォルダの検索）ページで、**Browse（参照）** ボタンをクリックし、```E:∕Virtual
    Machines∕WinSrv∕Arc```を参照し、**Select
    Folder（フォルダの選択**）ボタンをクリックします。

    ![ a computer ](./media/image9.png)

10. **Locate Folder** ページで、Nextボタンをクリックします。

    ![ a computer ](./media/image10.png)

11. **Select Virtual
    Machine**ページで **「Next」** ボタンをクリックします。

12. **Choose Import
    type（インポートタイプの選択**）」ページで、デフォルトのまま「Next」ボタンをクリックします。

    ![ a computer ](./media/image11.png)

13. **Connect
    Network」** ページで、**「Connection」** ドロップダウンから「**Microsoft
    Hyper-V Network
    Adapter**」を選択し、「Next」ボタンをクリックします。

    ![ a computer ](./media/image12.png)

14. **Complete Import
    Wizard** ページで詳細を確認し、**Finish**ボタンをクリックします。

    ![ a computer program ](./media/image13.png)

15. **WinSrv20224Arc**
    VMを右クリックし、**Start**オプションを選択します。

    ![ a computer ](./media/image14.png)

16. 再度、**WinSrv20224Arc**
    VMを右クリックし、**Connect**オプションを選択します。

    ![ a computer ](./media/image15.png)

17. 仮想マシン接続ウィンドウから **Ctrl+Alt+Delete** ボタンを押します。

    ![](./media/image16.png)

18. 以下の認証情報でログインしてください

    1.  ユーザー名 - ```Administrator```

    2.  パスワード - ```P@55w.rd1234```

    ![](./media/image17.png)

19. ログインに成功したことを確認してください。

**タスク 2 - スクリプトによる Azure Arc リソースの追加**

1.  **WinSrv20224Arc** VMにログインしたら、Edgeブラウザを開き、**Azure
    Portal**
    ```https://portal.azure.com/```に移動し、ラボから提供された認証情報を使用してサインインします。

2.  Azure Portal で「Type arc」と入力し、Azure Arc を選択する。

    ![ a computer ](./media/image18.png)

3.  **Manage resources across environmentsの下で、Add
    resources**ボタンをクリックします。

    ![](./media/image19.png)

4.  **Add Azure Arc resources**ページで、**「Add/Create** **」**
    ボタンをクリックし、「**Add a machine**」 を選択**します。**

    ![](./media/image20.png)

5.  **Add servers with Azure Arc** ページで、「**Add a single server**」
    の下にある 「**Generate script**  ボタンをクリックします。　

    ![ a computer ](./media/image21.png)

6.  **Add a server with Azure Arc** ページで、以下の詳細を入力します。

    <font color=darkred>

    > **リソースグループを作成する前に、エラーを避けるためにリージョンを選択してください。**

    </font>

    - 地域 - **West US**

    - リソースグループ - **Create new**  ```RG4ArcVM``` をクリックします。

    - オペレーティングシステム - **Windows**

    - SQL Serverを接続する - **チェックを外す**

    - **Download and run script buttonを**クリックします。

    ![](./media/image22.png)

7.  **Download**ボタンをクリックし、**Copy**ボタンもクリックしてください。

    ![](./media/image23.png)

8.  **「Start button**」**ボタンを**右クリックし、「**Windows PowerShell
    (Admin)」**を選択**します**。

    ![](./media/image24.png)

9.  **Windows PowerShell
    (Admin)** ウィンドウで、クリップボードからコピーしたスクリプトを貼り付けます。

    ![](./media/image25.png)

10. スクリプトは以下の画像のように起動します。

    ![](./media/image26.png)

11. ログインを求められたら、提供された認証情報でログインする。

    ![ a login page ](./media/image27.png)

12. 認証が成功したら、PowerShellウィンドウに戻る。

    ![A black text on a white background ](./media/image28.png)

13. 以下の画像のように、「**Connect Machine to
    Azure**」というメッセージが表示されるはずです。

    ![](./media/image29.png)

**タスク 3 - Arc サーバの管理**

1.  Lab VM に戻り、Azure Portal ```https://portal.azure.com```を開く。

2.  Azure Portal の検索で「arc」と入力し、**Azure Arc を**選択する。,

    ![](./media/image18.png)

3.  Azure Arc リソースで **Machines** を選択します。

    ![](./media/image30.png)

4.  マシン**WinSrv20224ArcがConnectedと**表示されるはずです。

    ![ a computer ](./media/image31.png)

5.  **WinSrv20224Arcを**クリックして詳細を開き、**Overview**ページで**Updatesを**クリックします。

    ![ a computer ](./media/image32.png)

6.  **Periodic
    assessment** **の**ドロップダウンから「**Enable」を**選択し、**「Save」** ボタンをクリックします。

    ![ a computer ](./media/image33.png)

    ![A close-up of a text ](./media/image34.png)

7.  **「Overview」**ページに戻り、**「Monitoring
    insights**」をクリックする。　

    ![ a computer ](./media/image35.png)

8.  **Insights**ページで、**Enable**ボタンをクリックする。

    ![ a computer ](./media/image36.png)

9.  **Monitoring
    configuration** ページで、**Configure**ボタンをクリックします。　

    ![ a computer ](./media/image37.png)

    > **注** - モニタリング・リソースの配置には、約5～10分かかります。

    ![ a computer ](./media/image38.png)

10. **「Overview」** ページに戻り、「Security」をクリックする。

    ![ a computer ](./media/image39.png)

    > **注- Microsoft Defender for**
    Cloudは以前に有効にしており、サーバーは最近オンボードにしたため、30分ほどで推奨を確認できるはずです。

11. 搭載されたサーバーの推奨が利用可能になると、以下の画像のように表示されるはずです。　

    ![ a computer ](./media/image40.png)

12. **Overview**ページに戻り、**Updatesを**クリックします。

    ![ a computer ](./media/image41.png)

13. 必要に応じて、「**Go to Updates using Azure Update
    Manager**」ボタンをクリックします。

14. **「Check for updates** 」ボタンをクリックします。

    ![ a computer ](./media/image42.png)

15. **Trigger assess nowの**メッセージで**OK**ボタンをクリックします。

    ![ a computer error ](./media/image43.png)

    ![A white background with black text ](./media/image44.png)

16. **「Refresh」** ボタンをクリックすると、「**Total
    updates** **」**セクションに**Assessment is in
    progress**表示されます。

    ![ a computer ](./media/image45.png)

17. アセスメントが完了したら、もう一度 **「Refresh」** ボタンをクリックしてください。

    ![A white text with black text ](./media/image46.png)

18. サーバー上で必要なアップデートの詳細を見ることができるはずです。

    ![ a computer ](./media/image47.png)

19. **One-time
    update** をクリックすると、サーバーのアップデートが開始されます。

    ![ a computer ](./media/image48.png)

20. Install one-time updatesページの「マシン」タブで**WinSrv20224Arc**
    VMを選択し、**「Next」** ボタンをクリックします。　

    ![ a computer ](./media/image49.png)

21. アップデートの詳細を確認し、 「Next」 ボタンをクリックします。

    ![ a computer ](./media/image50.png)

22. **「Properties」** タブで、**「Next」** ボタンをクリックします。

    ![ a computer ](./media/image51.png)

23. Review +
    Installタブで詳細を確認し、**Install**ボタンをクリックします。

    ![ a computer ](./media/image52.png)

    ![ a computer ](./media/image53.png)

24. 通知をクリックすると、アップデートの詳細が表示されます。

    ![ a computer ](./media/image54.png)

    ![ a computer ](./media/image55.png)

25. **「Overview」**ページに戻り、**「Monitoring
    insights**」をクリックします。　

    ![ a computer ](./media/image56.png)

26. **Analyze data** ボタンをクリックします。

    ![ a computer ](./media/image57.png)

27. これで、オンボード・サーバーの**Performance
    data** を見ることができるはずだ。

    ![ a computer ](./media/image58.png)

    ![ a computer ](./media/image59.png)

28. これで、オンプレミスの Windows Server
    のオンボード化に成功し、**Azure Arc** を使って Azure Portal から
    Server を管理できるようになりました。
