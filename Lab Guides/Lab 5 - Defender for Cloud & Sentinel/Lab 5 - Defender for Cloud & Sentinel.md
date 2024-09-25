# **ラボ 5 - Defender for Cloud と Sentinel。**

## タスク 1: Microsoft Defender for Cloud から VM で JIT を有効にする

1. **Azure Portal** ```https://portal.azure.com``` で、検索ボックスに ```Microsoft Defender for Cloud``` と入力し、**サービス** の下にある **Microsoft Defender for Cloud** をクリックします。

![](./media/image1.png)

3. **Microsoft Defender for Cloud \| 概要** ページの左側のペインで、**クラウド セキュリティ** セクションに移動し、**ワークロード保護** をクリックします。

![](./media/image2.png)

3. **Microsoft Defender for Cloud \| ワークロード保護** ページで、下の画像に示すように、下にスクロールして、**高度な保護** セクションの下にある **Just-in-time VM アクセス** をクリックします。

![](./media/image3.png)

4. **ジャストインタイム VM アクセス** ページで、**仮想マシン** セクションに移動し、**未構成** タブをクリックします。VM が表示されます - **PostgreSrv** は、**未構成** タブの下にリストされています。

![](./media/image4.png)

5. 使用可能な VM の 1 つを選択し、右側の **1 つの VM で JIT を有効にする** ボタンをクリックします。

![](./media/image5.png)

6. **JIT VM アクセス構成** ページで、**保存** をクリックします。

![](./media/image6.png)

7. 通知が表示されます - **ジャストインタイム VM アクセス構成が開始されました**。

![](./media/image7.png)

8. [**仮想マシン**] セクションの [**構成済み**] タブをクリックすると、**PostgreSrv** VM が [**構成済み**] タブに一覧表示されます。

![](./media/image8.png)

9. これで、この VM に接続するために、要求に応じてアクセスが許可されます。

![](./media/image9.png)

## タスク 2: セキュリティ アラートの生成と調査

1. [**Microsoft Defender for Cloud**] の [**全般**] セクションで、[**セキュリティ アラート**] を選択します。

![](./media/image10.png)

2. [**サンプル アラート**] ボタンをクリックしてアラートを生成します。

![](./media/image11.png)

3. [**サンプル アラートの作成**] ボタンをクリックします。

![](./media/image12.png)

![](./media/image13.png)

4. サンプルアラートが生成されます

![](./media/image14.png)

5. **更新** ボタンをクリックすると、サンプルアラートが表示されます。

![](./media/image15.png)

6. 調査したいアラートをクリックします

7. アラートの概要ペインで、次の詳細を調査します。

1. **重大度、ステータス、アクティビティ時間**

2. 検出されたアクティビティを正確に説明する **説明**

3. **影響を受けるリソース**

4. MITRE ATT&CK マトリックス上のアクティビティの **キルチェーンの意図**

8. 疑わしいアクティビティの調査に役立つ詳細情報については、**詳細を表示** ボタンをクリックします。

![](./media/image16.png)

9. **アラートの詳細** タブの情報を確認します。

![](./media/image17.png)

10. **アクションを実行** ボタンをクリックして、利用可能なオプションを確認します

![](./media/image18.png)

# 演習 2 – Sentinel のデプロイ

## タスク 1: Microsoft Sentinel ワークスペース

この演習では、Microsoft Sentinel ワークスペースの作成方法を説明します。

1. ```https://portal.azure.com``` に移動し、ラボ環境のラボ リソースで提供される **MOD 管理者** 資格情報を使用してログインします。

2. 上部の検索バーに ```Microsoft Sentinel``` と入力し、**Microsoft Sentinel** をクリックします。

![](./media/image19.png)

3. **Microsoft Sentinel** 画面で、左上にある **作成** をクリックします。

![](./media/image20.png)

4. **Microsoft Sentinel** を既存の **Log Analytics** **ワークスペース** に追加するか、新しいワークスペースを作成するかを選択できます。新しいワークスペースを作成するので、[新しいワークスペースの作成] をクリックします。

![](./media/image21.png)

5. **Log Analytics ワークスペースの作成** ページで、次のようにフォームに入力します:

1. サブスクリプション: **Azure Pass - スポンサーシップ**

2. リソース グループ: **新規作成** ``LAWResourceGroup`` をクリックします

3. ワークスペース名: ```SentWrkspcXXXXXX``` [**XXXXXX**
をランダムな数字に置き換えます\]

4. リージョン: **米国西部**

5. **確認と作成** をクリックします。

![](./media/image22.png)

6. 検証が完了したら、**作成** をクリックします。作成には数秒かかります。

![](./media/image23.png)

7. **Microsoft Sentinel をワークスペースに追加** ページにリダイレクトされるので、**更新** ボタンをクリックします。

![](./media/image24.png)

8. 作成したワークスペースを選択し、下部の [**追加**] をクリックします。

![](./media/image25.png)

9. 下の画像のように通知が表示されます。

![](./media/image26.png)

10. Microsoft Sentinel ワークスペースが使用できるようになりました。[**OK**] ボタンをクリックして続行します。

![](./media/image27.png)

## タスク 2: データ コネクタを有効にします。

この演習では、データ コネクタを有効にする方法を説明します。

1. ブラウザー タブで ```https://portal.azure.com/#view/Microsoft_AAD_UsersAndTenants/UserManagementMenuBlade/~/AllUsers```
に移動し、**テナント管理者アカウント** を選択します

![](./media/image28.png)

2. [管理] の下にある [割り当てられたロール] を選択し、[+ 割り当ての追加] をクリックします。

![](./media/image29.png)

3. [**セキュリティ管理者**] を検索して選択し、[**追加**] ボタンをクリックします。

![](./media/image30.png)

![](./media/image31.png)

4. Azure ポータルで
```https://portal.azure.com``` に移動し、``Microsoft Sentinel``` を検索して、**Microsoft Sentinel** をクリックします。

![](./media/image32.png)

5. **SentWrkspcXXXXXX** を選択します。

![](./media/image33.png)

6. 次に、**構成** セクションの **データ コネクタ** を選択します。

![](./media/image34.png)

7. **「コンテンツ ソース = ギャラリー コンテンツ」のデータ コネクタが削除されました。** というメッセージが表示されます。そのメッセージで、**ここをクリック** リンクを選択します。

![](./media/image35.png)

8. **すぐに使用できるコンテンツ集中化** ページで、**続行** をクリックします。

![](./media/image36.png)

9. **集中化を完了** ボタンをクリックします。

![](./media/image37.png)

10. 以下の画像に示すような通知が表示されます。

![](./media/image38.png)

11. 上部から **Microsoft Sentinel** のリンクをクリックするか、Sentinel ページに戻ります。

![](./media/image39.png)

13. **更新** ボタンをクリックすると、いくつかのコネクタが表示されます。

![](./media/image40.png)

<font color=darkgreen>

> **注** - コネクタがインストールされない場合もありますが、ラボを先に進めても問題ありません。

</font>

14. **コンテンツ管理** の下にある **コンテンツ ハブ** をクリックします

![](./media/image41.png)

15. コンテンツ ハブ ページで ```Azure Activity``` を検索し、**Azure Activity** コンテンツを選択して、**インストール** ボタンをクリックします

![](./media/image42.png)

16. コンテンツ ハブ ページで ```Microsoft Defender for Cloud``` を検索し、**Microsoft Defender for Cloud** コンテンツを選択して、**インストール** ボタンをクリックします

![](./media/image43.png)

## タスク 3: Azure アクティビティ データ コネクタを有効にする

この演習では、Azure アクティビティ データ コネクタを有効にする方法を説明します。

このコネクタは、Azure サブスクリプションで実行されたアクションのすべての監査イベントを Microsoft Sentinel ワークスペースに取り込みます。

1. **Microsoft Sentinel** ページで、**構成** セクションの **データ コネクタ** をクリックします。

![](./media/image44.png)

2. データ コネクタ画面で、検索バーに「``activity```」と入力し、**Azure アクティビティ** コネクタを選択して、**コネクタ ページを開く** をクリックします。

![](./media/image45.png)

3. **Azure アクティビティ コネクタ** ページで、オプション番号 **2. に移動します。

診断設定の新しいパイプラインを使用してサブスクリプションを接続します**。 この方法は Azure Policy を活用し、古い方法と比較して多くの改善をもたらします (これらの改善の詳細については、こちらを参照してください)。 **Azure Policy 割り当ての起動** ウィザードをクリックすると、ポリシー作成ページにリダイレクトされます。

![](./media/image46.png)

4. **スコープ** の選択で、**Azure Pass – スポンサーシップ** を選択します。

**選択** をクリックします。

![](./media/image47.png)

5. **パラメーター** タブに移動します。**プライマリ Log Analytics ワークスペース** で、**MicrosoftSentinelWorkspace** を選択します。

![](./media/image48.png)

6. **修復** タブで、**修復タスクの作成** の横にあるチェック ボックスをオンにして、**確認と作成** ボタンをクリックします。

![](./media/image49.png)

7. **確認と作成** タブで、**作成** ボタンをクリックします。

![](./media/image50.png)

8. **通知** ペインに、「**ロール割り当ての作成が成功しました**」、「**修復タスクの作成が成功しました**」、および「**ポリシー割り当ての作成が成功しました**」という通知が表示されます。

![](./media/image51.png)

9. **Azure アクティビティ コネクタ** ページで、接続状態を確認できます。

![](./media/image52.png)

<font color=darkblue>

> **注**: コネクタがすぐに「接続済み」と緑色で表示されない場合、プロセスが完了するまでに約 30 分かかります。

</font>

10. 次の演習に進み、30 分後にもう一度確認してください。

## タスク 4: Microsoft Defender for Cloud データ コネクタを有効にします。

この演習では、Microsoft Defender for Cloud データ コネクタを有効にする方法を説明します。このコネクタを使用すると、Microsoft Defender for Cloud から Microsoft Sentinel にセキュリティ アラートをストリーミングできるため、ワークブックで Defender データを表示したり、クエリを実行してアラートを生成したり、インシデントを調査して対応したりできます。

1. **Microsoft Sentinel** ページで、**構成** セクションの **データ コネクタ** をクリックします。

![](./media/image44.png)

2. **データ コネクタ** 画面で、検索バーに「``tenant```」と入力し、**テナントベースの Microsoft Defender for Cloud** **(プレビュー)** コネクタを選択して、**コネクタ ページを開く** をクリックします。

![](./media/image53.png)

> **注意** - **データ コネクタが見つかりません** というエラーが表示された場合は、**コンテンツ ハブ** に移動して、**Microsoft Defender for Cloud Connector** を再度インストールしてください。

![](./media/image68.png)

![](./media/image69.png)

3. **テナントベースの Microsoft Defender for Cloud** **(プレビュー)**
コネクタ ページの **構成** セクションで、
**接続** ボタンをクリックします。

![](./media/image54.png)

4. **正常に接続されました** という通知が表示されます。

![](./media/image55.png)

5. 1～2 分待ってからページを更新すると、コネクタのステータスも **接続済み** に更新されます。

![](./media/image56.png)

6. **データ コネクタ** 画面に戻り、検索バーに ```subscription``` と入力し、**サブスクリプション ベースの Microsoft Defender for Cloud** **(レガシ)** コネクタを選択して、**コネクタ ページを開く** をクリックします。

![](./media/image57.png)

7. **サブスクリプションベースの Microsoft Defender for Cloud**
**(レガシ)** コネクタ ページの **構成** セクションで、**Azure Pass – スポンサーシップ** サブスクリプションを選択し、**接続** ボタンをクリックします。

![](./media/image58.png)

8. **正常に接続されました** という通知が表示されます。

![](./media/image59.png)

9. コネクタのステータスも **接続済み** に更新されます。

![](./media/image60.png)

# 演習 3 - 統合

Defend for Cloud コネクタをインストールしたので、サンプル アラートを使用して生成された Microsoft Defender for Cloud からのインシデントを表示できるはずです。

1. **Microsoft Sentinel** ページで、脅威管理の下にある **インシデント** をクリックします。

![](./media/image61.png)

2. **Microsoft Defender for Cloud** コネクタを有効にしたばかりなので、インシデントが表示されるまでに約 20 ～ 30 分かかります。

3. **全般** の下にある **概要** をクリックし、**新しい概要** スイッチを **オフ** に切り替えます。

![](./media/image62.png)

4. スイッチをオフにすると、Microsoft Defender for Cloud からの **サンプル イベント** を表示できるはずです。

![](./media/image63.png)

5. **SecurityAlerts** をクリックします

![](./media/image64.png)

6. Log Analytic ワークスペースが開き、**Microsoft Defender for Cloud** から生成および同期された **Alerts** のすべてのログが一覧表示されます。

![](./media/image65.png)

7. 任意の **Alerts** をクリックして展開し、その詳細を一覧表示します。

![](./media/image66.png)

8. 展開されたアラートの詳細が表示されます。

<font color=darkred>

1. TimeGenerated [UTC]

2. Displayname

3. AlertName

4. AlertSeverity

5. 検出されたアクティビティを正確に説明する説明

6. ProviderName – Azure Security Center – Microsoft Defender for Cloud の旧称

7. RemeditalSteps

8. 追加情報を含むその他の行。
</font>

![](./media/image67.png)  
