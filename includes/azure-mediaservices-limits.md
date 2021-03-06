>[!NOTE]
>固定されていないリソースについては、サポート チケットを開いてクォータの増加を依頼できます。 上限を高くするために追加の Azure Media Services アカウントを作成することは**しないでください**。

| リソース | 既定の制限 | 
| --- | --- | 
| 1 つのサブスクリプション内の Azure Media Services (AMS) アカウント数 | 25 (固定) |
| AMS アカウントあたりのアセット数 | 1,000,000|
| ジョブあたりのチェーン タスク数 | 30 (固定) |
| タスクあたりのアセット数 | 50 |
| ジョブあたりのアセット数 | 100 |
| AMS アカウントあたりのジョブ数 | 50,000<sup>2</sup> |
| 一度に 1 つの資産に関連付けられる一意のロケーター数 | 5<sup>4</sup> |
| AMS アカウントあたりのライブ チャネル数 |5|
| チャネルあたりの停止状態のプログラム数 |50|
| チャネルあたりの実行状態のプログラム数  |3|
| AMS アカウントあたりの実行状態のストリーミング エンドポイント数|2|
| ストリーミング エンドポイントあたりのストリーミング ユニット数  |10 |
| AMS アカウントあたりのメディア占有ユニット数 (RU) |25 (S1、S2)<br/>10 (S3) <sup>1</sup> | 
| ストレージ アカウント | 1,000<sup>5</sup> (固定) |
| ポリシー | |1,000,000<sup>6</sup> |
 
<sup>1</sup> S3 RU はインド西部では使用できません。

<sup>2</sup> この数には、キューに置かれたジョブ、終了したジョブ、アクティブなジョブ、取り消されたジョブが含まれています。 これには、削除されたジョブは含まれません。 **IJob.Delete** または **DELETE** HTTP 要求を使用して古いジョブを削除できます。

<sup>3</sup> Job エンティティの一覧を要求すると、要求ごとに最大 1,000 件が返されます。 送信したすべてのジョブを追跡する必要がある場合は、「 [OData システム クエリ オプション](http://msdn.microsoft.com/library/gg309461.aspx)」の説明に従って top/skip を使うことができます。

<sup>4</sup>ロケーターはユーザーごとのアクセス制御を管理するようには設計されていません。 個々のユーザーに異なるアクセス権限を付与するには、デジタル著作権管理 (DRM) ソリューションを使用します。 詳細については、 [こちらの](../articles/media-services/media-services-content-protection-overview.md) セクションを参照してください。

<sup>5</sup> ストレージ アカウントは、同じ Azure サブスクリプションからのものである必要があります。

<sup>6</sup> さまざまな AMS ポリシー (ロケーター ポリシーや ContentKeyAuthorizationPolicy など) に 1,000,000 ポリシーの制限があります。 

>[!NOTE]
> 常に同じ日数、アクセス許可などを使用する場合は、同じポリシー ID を使用する必要があります。



<!--HONumber=Jan17_HO2-->


