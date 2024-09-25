# ラボ3 - PostgreSQLをAzureに移行する。

**目的**

ラボでは、**PostgreSQLデータベースを**ホストする仮想マシンをデプロイし、必要な**PostgreSQLインフラストラクチャを**作成し、**Azure
Database for Postgres Flexible
Server（移行**）を使用してPostgreSQLデータベースを移行します**。     

![](./media/image1.png)

**タスク1 -
オンプレミス環境のPostgreSQLデータベースをホストする仮想マシンをデプロイします。**

**Ubuntu 22.0.4.4 LTS** VMをデプロイし、そこに**PostgreSQL Server
16を**インストールし、マイグレーションに使用するSample
Databaseを作成します。

1.  Azure Portal ```https://portal.azure.com``` から Azure Cloud Shell
    を開く。

    ![AA screenshot of a computer Description automatically
](./media/image2.png)

2.  **PowerShell**ボタンをクリックします。

    ![AA screenshot of a computer error Description automatically
](./media/image3.png)

3.  **Getting started\]** ウィンドウで、**\[Mount storage account\]**
    のラジオボタンを選択し、\[**Azure Pass - Sponsorship\]**
    サブスクリプションを選択して **\[Apply\]** ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image4.png)

4.  ストレージアカウントのマウント」ウィンドウで、「**We will create
    storage account for
    you**」ラジオボタンを選択し、「**Next**」をクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image5.png)

5.  配備が完了するまで待つ

    ![AA screenshot of a computer program Description automatically
](./media/image6.png)

6.  Cloud Shell
    PowerShellウィンドウで以下のコマンドを入力して変数を設定し、PostgreSQLサーバのインストールに使用するVMを作成します。

    ```$cred = Get-Credential```

7.  認証情報の入力を求められたら、以下を入力してください。

    ユーザー - ```postgres```

    パスワード - ```P@55w.rd1234```

    ![AA screenshot of a computer screen Description automatically
](./media/image7.png)

8.  以下のコマンドを入力し、リソース・グループを作成する。

    ```New-AzResourceGroup-ResourceGroupName "PostgresRG"-Location "WestUS```
    ![](./media/image8.png)

9.  以下のコマンドを入力して、Windows Server 2019 Datacenter VMをデプロイする。

    ```
    New-AzVm `
        -ResourceGroupName "PostgresRG" `
        -Name "PostgresSrv" `
        -Location "WestUS" `
        -VirtualNetworkName "PGVnet" `
        -SubnetName "PGSubnet" `
        -SecurityGroupName "PostgresNSG" `
        -Securitytype "Standard" `
        -PublicIpAddressName "PostgresSrvIP" `
        -ImageName "Canonical:0001-com-ubuntu-server-jammy:22_04-lts-gen2:latest" `
        -Credential $cred `
        -Size "Standard_b2ms"
    ```

    ![AA computer screen shot of a computer Description automatically
](./media/image9.png)

10. デプロイが完了すると、以下のように表示される。

    ![AA blue screen with white text Description automatically
](./media/image10.png)

11. 以下のコマンドを実行してUbuntu
    VMに接続し、前のコマンドの出力から**FullyQualifiedDomainNameを**使用してコマンドを置き換える.

    ![AA screen shot of a computer Description automatically
](./media/image11.png)

    ```ssh postgres@FullyQualifiedDomainName```

    ![AA screen shot of a computer Description automatically
](./media/image12.png)

12. 続行を求められたら「**yes**」と入力し、配備時に指定されたパスワードを入力する -
    ```P@55w.rd1234```

13. Ubuntuサーバーへの接続に成功するはずです。

    ![AA screenshot of a computer screen Description automatically
](./media/image13.png)

14. では、Ubuntu VMに**PostgreSQL
    ver.16を**にインストールし、以下のコマンドを実行して自動リポジトリの設定を行います。


    ```sudo apt install -y postgresql-common```

    ```sudo /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh```

    ![AA computer screen shot of a blue screen Description automatically
](./media/image14.png)

    ![AA blue screen with white text Description automatically
](./media/image15.png)

15. キーボードのEnterキーを押して続ける。

    ![AA screenshot of a computer program Description automatically
](./media/image16.png)

    ![AA screen shot of a computer screen Description automatically
](./media/image17.png)

16. 以下のコマンドを実行して、**repository signing
    key** **をインポート**する。


    ```sudo apt install curl ca-certificates```

    ```sudo install -d /usr/share/postgresql-common/pgdg```

    ```sudo curl -o /usr/share/postgresql-common/pgdg/apt.postgresql.org.asc --fail https://www.postgresql.org/media/keys/ACCC4CF8.asc```

    ![AA computer screen shot of a blue screen Description automatically
](./media/image18.png)

17. 以下のコマンドを実行して、**リポジトリ設定ファイルを作成**します。

    ```sudo apt update```

    ```sudo apt install gnupg2 wget```

    ```sudo sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list'```

    ```curl -fsSL https://www.postgresql.org/media/keys/ACCC4CF8.asc | sudo gpg --dearmor -o /etc/apt/trusted.gpg.d/postgresql.gpg```

    ![A](./media/image19.png)

18. 以下のコマンドを実行して、**パッケージ・リストを更新**する。

    ```sudo apt update```

    ![AA computer screen with white text Description automatically
](./media/image20.png)

19. 以下のコマンドを実行して、**最新バージョンのPostgreSQLをインストール**する。

    ```sudo apt install postgresql-16 postgresql-contrib-16```

    ![AA computer screen shot of a blue screen Description automatically](./media/image21.png)

    > **注意 - インストールは1-2分で完了します。**

    ![AA blue screen with white text Description automatically](./media/image22.png)


    ![AA blue screen with white text Description automatically](./media/image23.png)

20. インストールが完了したら、以下のコマンドを入力して PSQL
    ユーティリティを起動します。

    ```Psql```

    ![AA blue screen with white text Description automatically
](./media/image24.png)

21. psqlで**postgres**アカウントのパスワードを設定する。

    ```\password postgres```

22. パスワードをpostgresと入力する。ポストグレスとしてまた入力する。

    ![AA computer screen shot of a blue screen Description automatically
](./media/image25.png)

23. ここで、リモートからアクセスするすべてのPostgreSQLに対して、ネットワークとその他のパーミッションを設定します。

24. 以下のコマンドを実行して、**postgresql.conf**ファイルにアクセスする。

    ```\q```

    ```sudo nano /etc/postgresql/16/main/postgresql.conf```

25. ファイルを開いたら下にスクロールし、以下のように設定を更新する。

    **\[Connection Settings\]で#を削除し、listen_addresses =
    '\*'に変更する。**

    ![A](./media/image26.png)

    **WRITE-AHEAD LOGから#を削除し、wal_level = logicalに変更する。**

    ![AA computer screen with white text Description automatically
](./media/image27.png)

26. 上記の変更が完了したら、**Ctrl + Xを**押します。

    ![AA screenshot of a computer program Description automatically
](./media/image28.png)

27. タイプ**Yを**押し、エンターキーで確定する。

28. 以下のコマンドを実行して**pg_hba.conf**ファイルにアクセスする。

    ```sudo nano /etc/postgresql/16/main/pg_hba.conf```

29. ファイルを開いたら下にスクロールし、ファイルの一番下に以下の行を追加する。

    ```
    host all all 0.0.0.0/0 md5
    host all all ::/0 md5
    ```

    ![](./media/image29.png)

30. 上記の変更が完了したら、**Ctrl + X**を押します。

    ![AA blue screen with white text Description automatically
](./media/image30.png)

31. タイプ**Yを**押し、エンターキーで確定する。

32. 以下のコマンドを実行して、PostgreSQLサービスを再起動する。

    ```sudo service postgresql restart```

    ![AA blue screen with white text Description automatically
](./media/image31.png)

33. Azure Portalで、「Resource groups」を検索して選択します！

    ![AA screenshot of a computer Description automatically
](./media/image32.png)

34. リソースグループのリストから**PostgresRGを**選択し、VM -
    **PostgresSrvを**選択します。

35. **PostgresSrv**ページで、**Networking設定を**選択し、**+ Create port
    ruleを**クリックし、**Inbound port Ruleを**選択します。

    ![AA screenshot of a computer Description automatically
](./media/image33.png)

36. **Add inbound security
    rules（受信セキュリティルールの追加**）ページで、service（サービス）のドロップダウンから**PostgreSQLを**選択し、**Add**ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image34.png)

37. 下の画像のような通知が届くはずです。

    ![AA white background with blue text Description automatically
](./media/image35.png)

38. これでPostgreSQLサーバーにリモートからアクセスできるようになりました。

**タスク 2 - オンプレミス環境の PostgreSQL データベースを作成します。**

1.  それでは、移行に使用するPostgreSQLサーバーにサンプル・データベースをインポートします。

2.  DVDレンタルデータベースには15のテーブルがあります。

    ![APostgreSQL Sample Database Diagram](./media/image36.png)

3.  Azure PortalからAzure Cloud Shellを開く。

    ![AA screenshot of a computer Description automatically
](./media/image2.png)

4.  クラウドシェルがBashで起動したことを確認し、以下のコマンドを実行して**PostgresSrv**
    VMに接続します。

    ```ssh postgres@ServerDNSName```

    ![AA screenshot of a computer Description automatically
](./media/image37.png)

5.  続行を求められたら「**yes**」と入力し、パスワード-
    ```P@55w.rd1234```を入力する.

6.  Ubuntuサーバーへの接続に成功するはずです。

    ![AA screenshot of a computer Description automatically
](./media/image38.png)

7.  プロンプト**postgres@PostgresSrvで**以下のコマンドを実行し、データベースのリストアに使用するファイルをコピーする**フォルダを**作成してください。

    ```mkdir dvdrentalbkp```

    ![A](./media/image39.png)

8.  Lab VM 上で、\[Start\] メニューを右ク リ ッ ク し、\[Windows
    Terminal (admin)\] を選択します。

    ![AA screenshot of a computer Description automatically
](./media/image40.png)

9.  Windows
    PowerShellウィンドウで、PostgreSQLデータベースのバックアップを**PostgresSrv**上のフォルダ**dvdrentalbkpに**コピーするコマンドを実行します。

    <font color=red>

    >**注意** - コマンドを実行する前に、実行するコマンドを **Ububtu Server VM
    の FQDN** に置き換えてください。
    </font>


    ```scp "C:\Labfiles\dvdrental.tar"postgres@FQDNofUbubtuServerVM:"dvdrentalbkp"```

    続行を求められたら「**yes**」と入力し、パスワードを入力する -
    ```P@55w.rd1234```

    ![AA computer screen with white text Description automatically
](./media/image41.png)


    <font color=blue>

    > **注意** -
**dvdrental.tar**ファイルが存在しない場合は、**dvdrental.tar**ファイルを-++https://github.com/technofocus-pte/migrt2Innovregdepth/raw/main/Lab%20Guides/Labfiles/dvdrental.tar
からダウンロードし、**C:˶Labfiles```に置いて**ください。
</font>

10. プロンプト**postgres@PostgresSrvの**タブに戻り、以下のコマンドを実行してPSQLを起動します。

    ```psql```

    ![](./media/image42.png)

11. **psql**プロンプトで以下のコマンドを実行してデータベースを作成する。

    ``` CREATE DATABASE dvdrental;```

    ![AA black screen with yellow text Description automatically
](./media/image43.png)

    ```\q```

    ![AA black background with green text Description automatically
](./media/image44.png)

12. **postgres@PostgresSrv**プロンプトに戻って以下のコマンドを入力し、新しく作成したデータベースにバックアップをリストアします。

    ```cd dvdrentalbkp```

    ```pg_restore -U postgres -d dvdrental "dvdrental.tar "```

    ![A](./media/image45.png)

**注意** -
エラーや警告メッセージが表示された場合は、それらを無視しても問題ありません。空のデータベースが
15 個のテーブルで更新されます。

13. 以下のコマンドを実行することで、データベースの詳細を確認することができます。

    ```psql```

    ```\c dvdrental```

    ![](./media/image46.png)

    ```dt```

    ![](./media/image47.png)

**タスク3 - PostgreSQL flexible Server用のAzureデータベースを作成する**

1.  Edge ブラウザを開き、Azure Portal ```https://portal.azure.com```
    に移動する。　

2.  postgresを検索し、**Azure Database for PostgreSQL flexible
    Serversを**選択します。

    ![AA screenshot of a computer Description automatically
](./media/image48.png)

3.  **「+ Create**」をクリック

    ![AA screenshot of a computer Description automatically
](./media/image49.png)

4.  **Basics** タブの **New Azure Database for PostgreSQL Flexible
    Server** ページで、以下の詳細を入力します。

    - リソースグループ - 新規作成をクリックし、名前を指定 -
      ```RG4AzPGDb```+

    - サーバー名 - ```ad4pfssrvXXXXX``` substitute XXXXX with random
      number

    - 地域 - **West US**

    - PostgreSQL バージョン - **16**

    - ワークロード・タイプ – **Production**

    ![AA screenshot of a computer Description automatically
](./media/image50.png)

    - 高可用性 - **無効（Disabled）**

    - 認証方法 - **PostgreSQL認証のみ**

    - 管理者ユーザー名 - ```postgres```

    - パスワード - ```P@55w.rd1234```

    - パスワードの確認 - ```P@55w.rd1234```

    - **Next: Networking \>**クリックします**。**

    ![](./media/image51.png)

5.  **「Networking」**タブで、\[**Allow public access from any Azure
    services within Azure to this
    server**\]のチェックボックスを有効にし、**\[+ Add Client IP
    address**\]をクリックして、**PostgresSrvのパブリックIP**アドレスを追加し、**\[Review +
    create\]**ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image37.png)

    ![AA screenshot of a computer Description automatically
](./media/image52.png)

6.  詳細を確認し、**Create**ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image53.png)

7.  配備が始まる。

    ![AA screenshot of a computer Description automatically
](./media/image54.png)

**注意** - 配置の完了には約10分かかります。

8.  デプロイが完了したら、**Go to resource**ボタンをクリックする。

    ![AA screenshot of a computer Description automatically
](./media/image55.png)

**タスク4 - PostgreSQLデータベースをAzure Database for
PostgreSQLフレキシブルサーバーに移行する（移行）**

1.  **Azure Database for PostgreSQL flexible server** **の Overview**
    ページが開くはずです。

    ![AA screenshot of a computer Description automatically
](./media/image56.png)

2.  概要ページを確認し、さまざまなタブをチェックする

    ![AA screenshot of a computer Description automatically
](./media/image57.png)

3.  **「Settings」**で**「データベース」**を選択すると、3つのデータベースが表示されます。

    ![AA screenshot of a computer Description automatically
](./media/image58.png)

4.  **「Migration」を**クリックし、**「+ Create** 」ボタンを選択する。

    ![AA screenshot of a computer Description automatically
](./media/image59.png)

5.  **「Setup**」タブの**「Migrate PostgreSQL to Azure Database for
    PostgreSQL Flexible
    Server**」ページで、以下の情報を入力し、「**Next: Select Runtime
    Server\>**」をクリックします。

    - 移行名 - ```PostgreSQLToAzurePG```

    - ソースサーバー - **On-premise Server**

    - 移行オプション - **Validate and Migrate**

    - 移行モード - **Online**

    ![AA screenshot of a computer Description automatically
](./media/image60.png)

6.  **Select Runtime Server**タブで**Next: Connect to
    source\>を**クリックする。

    ![AA screenshot of a computer Description automatically
](./media/image61.png)

7.  **Connect to sourceタブで**、以下の詳細を入力し、**Next : Select
    migration target\>を**クリックする**。**

    - サーバ名 - **Public IP address / DNS name of PostgresSrv VM**

    - ポート - ```5432```

    - サーバー管理者ログイン名 - ```postgres```

    - パスワード - ```postgres```

    - SSLモード – **Prefer**

    - 接続テスト - **Connect to source**をクリック

    > **テスト接続が成功するまで待つ。**

    ![AA screenshot of a computer Description automatically
](./media/image62.png)

8.  **Select migration target**タブで、以下の詳細を入力します。

    - パスワード - ```P@55w.rd1234```

    - 接続テスト - **Connect to source**をクリック

    > **テスト接続が成功するまで待つ。**

    - **Next : Select database(s) for migration**をクリックします**。**

    ![AA screenshot of a computer Description automatically
](./media/image63.png)

9.  [Select database(s) for migration」タブで\[Database -
    **dvdrental**\]を選択し、\[Next: Summary\>\]**をクリックします。

    ![](./media/image64.png)

10. 「Summary\]**タンで、表示されている情報を確認し、\[Start
    Validation and Migration\]**ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image65.png)

11. 移行ページで、**PostgreSQLToAzurePG**リンクをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image66.png)

12. **PostgreSQLToAzurePG**
    ページで、更新ボタンをクリックして更新を確認します。

    ![AA screenshot of a computer Description automatically
](./media/image67.png)

13. データベース名**dvdrentalを**クリックします。

    ![AA screenshot of a computer Description automatically
](./media/image68.png)

14. **Validation**タブで、バリデーションタスクの詳細を見ることができるはずです。

    ![AA screenshot of a computer Description automatically
](./media/image69.png)

15. **Migration**タブでは、移行ステータスがキューに入れられた状態で表示されます。

    ![AA screenshot of a computer Description automatically
](./media/image70.png)

16. **PostgreSQLToAzurePG**
    ページで、もう一度更新ボタンをクリックすると、移行タスクも完了し、**Waiting
    for User Action** と表示されるので、**Cutover**
    ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image71.png)

17. Please perform the following steps manually before doing the
    cutoverというプロンプトが表示されたら、「**Yes**」ボタンをクリックします。    ![AA
    screenshot of a computer Description automatically
    ](./media/image72.png)

    ![AA white background with black text Description automatically
](./media/image73.png)

18. **PostgreSQLToAzurePG**ページで、更新ボタンを再度クリックすると、**Migration
    details**下にカットオーバー中ステータスが表示されるはずです。

    ![AA screenshot of a computer Description automatically
](./media/image74.png)

19. カットオーバーが**完了**したら、**PostgreSQLToAzurePG**ブレードを閉じます。

    ![AA screenshot of a computer Description automatically
](./media/image75.png)

20. **Migration**ページに戻ると、PostgreSQLデータベースの移行が**Succeeded**ことがわかります。　

    ![AA screenshot of a computer Description automatically
](./media/image76.png)

21. 「設定\]で**\[Databases\]**を選択し、**dvdrentalを**選択して**\[Connect\]**ボタンをクリックします。

    ![AA screenshot of a computer Description automatically
](./media/image77.png)

22. クラウドシェルが開くとパスワードの入力を求められますので、パスワードを
    ```P@55w.rd1234``` と入力してください。

    ![AA screenshot of a computer Description automatically
](./media/image78.png)

23. データベースへの接続に成功すると、**devrental=\>と**表示される。

24. 以下のコマンドを実行して、ターゲット・データベースのテーブルを一覧表示する。

    ```dt```

    ![AA screenshot of a computer Description automatically
](./media/image79.png)

    > **注** - これらのテーブルは、ソース・データベースと同じです。

    ![AA diagram of a computer Description automatically
](./media/image36.png)

    <font color=darkgreen>

    > これで、オンプレミスのPostgreSQLデータベースをAzure Database for PostgreSQL Flexible Serverに移行することができました。

    </font>


**概要**

ラボでは、PostgreSQLデータベースをホストする仮想マシンをデプロイし、**Azure
Database for Postgres Flexible Server
(Migration)**を使用してPostgreSQLデータベースを移行しました**。**

![AA screenshot of a computer Description automatically
](./media/image47.png)

![AA screenshot of a computer Description automatically
](./media/image75.png)
