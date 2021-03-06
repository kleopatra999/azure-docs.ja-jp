---
title: ".NET を使用したオンデマンド コンテンツ配信の概要 | Microsoft Docs"
description: "このチュートリアルでは、Azure Media Services と .NET を使用したオンデマンド コンテンツ配信アプリケーションの実装手順を紹介します。"
services: media-services
documentationcenter: 
author: Juliako
manager: erikre
editor: 
ms.assetid: 388b8928-9aa9-46b1-b60a-a918da75bd7b
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: dotnet
ms.topic: hero-article
ms.date: 12/26/2016
ms.author: juliako
translationtype: Human Translation
ms.sourcegitcommit: f01cd8d3a68776dd12d2930def1641411e6a4994
ms.openlocfilehash: a9f77a58cdb13c357b6c3734bd9e3efa4ff5087b


---

# <a name="get-started-with-delivering-content-on-demand-using-net-sdk"></a>.NET SDK を使用したオンデマンド コンテンツ配信の概要
[!INCLUDE [media-services-selector-get-started](../../includes/media-services-selector-get-started.md)]

> [!NOTE]
> このチュートリアルを完了するには、Azure アカウントが必要です。 詳細については、 [Azure の無料試用版サイト](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)を参照してください。
>
>

## <a name="overview"></a>Overview
このチュートリアルでは、Azure Media Services (AMS) SDK for .NET を使用したビデオ オン デマンド (VoD) コンテンツ配信アプリケーションの実装手順について説明します。

チュートリアルでは Media Services の基本的なワークフローや、Media Services 開発に必要となる一般的なプログラミング オブジェクトやタスクについて紹介します。 このチュートリアルを完了すると、サンプル メディア ファイルをアップロード、エンコード、ダウンロードして、ストリーミングやプログレッシブ ダウンロードを実行できます。

### <a name="ams-model"></a>AMS モデル

次の図は、Media Services OData モデルに対する VoD アプリケーションの開発時に最もよく使用されるオブジェクトの一部を示しています。

画像をクリックすると、フル サイズで表示されます。  

<a href="https://docs.microsoft.com/en-us/azure/media-services/media/media-services-dotnet-get-started/media-services-overview-object-model.png" target="_blank"><img src="./media/media-services-dotnet-get-started/media-services-overview-object-model-small.png"></a> 

モデル全体は、[こちら](https://media.windows.net/API/$metadata?api-version=2.14)で確認できます。  

## <a name="what-youll-learn"></a>学習内容

このチュートリアルでは、次のタスクの実行方法を示します。

1. Media Services アカウントの作成 (Azure ポータルを使用)
2. ストリーミング エンドポイントの構成 (Azure Portal を使用)
3. Visual Studio プロジェクトの作成と構成
4. Media Services アカウントに接続します。
5. 新しい資産を作成し、ビデオ ファイルをアップロードします。
6. 一連のアダプティブ ビットレート MP4 ファイルにソース ファイルをエンコードします。
7. 資産を発行してストリーミング URL とプログレッシブ ダウンロード URL を取得します。
8. コンテンツを再生することでテストします。

## <a name="prerequisites"></a>前提条件
チュートリアルを完了するには次のものが必要です。

* このチュートリアルを完了するには、Azure アカウントが必要です。

    アカウントがない場合は、無料試用版のアカウントを数分で作成することができます。 詳細については、 [Azure の無料試用版サイト](https://azure.microsoft.com/pricing/free-trial/?WT.mc_id=A261C142F)を参照してください。 Azure の有料サービスを試用できるクレジットが提供されます。 このクレジットを使い切ってもアカウントは維持されるため、Azure App Service の Web Apps 機能など、無料の Azure サービスと機能を利用できます。
* オペレーティング システム: Windows 8 以降、Windows 2008 R2、Windows 7。
* .NET Framework 4.0 以降
* Visual Studio 2010 SP1 (Professional、Premium、Ultimate、または Express) 以降のバージョン。

## <a name="create-an-azure-media-services-account-using-the-azure-portal"></a>Azure Portal を使用した Azure Media Services アカウントの作成
このセクションでは、AMS アカウントを作成する方法について説明します。

1. [Azure ポータル](https://portal.azure.com/)にログインします。
2. **[+新規]** > **[メディア + CDN]** > **[Media Services]** の順にクリックします。

    ![Media Services Create](./media/media-services-portal-vod-get-started/media-services-new1.png)
3. **[CREATE MEDIA SERVICES ACCOUNT (Media Services アカウントの作成)]** に必要な値を入力します。

    ![Media Services Create](./media/media-services-portal-vod-get-started/media-services-new3.png)

   1. **[アカウント名]** に新しい AMS アカウントの名前を入力します。 Media Services アカウント名に使用できる文字は、小文字または数字のみで、空白を含めることはできません。長さは 3 ～ 24 文字です。
   2. [サブスクリプション] ボックスで、アクセス権のある別の Azure サブスクリプションを選択します。
   3. **[リソース グループ]**ボックスで、新規または既存のリソースを選択します。  リソース グループとは、ライフサイクル、アクセス許可、ポリシーを共有するリソースの集まりです。 [こちら](../azure-resource-manager/resource-group-overview.md#resource-groups)をご覧ください。
   4. **[場所]** ボックスで、この Media Services アカウントのメディアとメタデータのレコードを保存するリージョンを選択します。 このリージョンでメディアの処理とストリーミングが行われます。 ドロップダウン リストのボックスには、利用可能な Media Services リージョンのみが表示されます。
   5. **[ストレージ アカウント]**ボックスで、Media Services アカウントのメディア コンテンツの BLOB ストレージとなるストレージ アカウントを選択します。 Media Services アカウントと同じリージョンにある既存のストレージ アカウントを選択することも、ストレージ アカウントを作成することもできます。 新しいストレージ アカウントは同じリージョンに作成されます。 ストレージ アカウントの命名規則は、Media Services アカウントと同じです。

       ストレージの詳細については、 [こちら](../storage/storage-introduction.md)を参照してください。
   6. **[ダッシュボードにピン留めする]** チェック ボックスをオンにして、アカウントのデプロイの進行状況を確認します。
4. フォームの下部にある **[作成]** をクリックします。

    アカウントの作成に成功すると、ステータスが **[実行中]**に変化します。

    ![Media Services settings](./media/media-services-portal-vod-get-started/media-services-settings.png)

    AMS アカウントを管理するには (ビデオのアップロード、資産のエンコード、ジョブの進行の監視など)、 **[設定]** ウィンドウを使用します。

## <a name="configure-streaming-endpoints-using-the-azure-portal"></a>Azure Portal を使用したストリーミング エンドポイントの構成
クライアントに対するアダプティブ ビットレート ストリーミングでのビデオ配信は、Azure Media Services の代表的な用途の 1 つです。 Media Services でサポートされるアダプティブ ビットレート ストリーミング テクノロジは、HTTP ライブ ストリーミング (HLS)、Smooth Streaming、および MPEG DASH です。

Media Services にはダイナミック パッケージ機能があり、アダプティブ ビットレート MP4 でエンコードされたコンテンツを、Media Services でサポートされるストリーミング形式 (MPEG DASH、HLS、Smooth Streaming) でそのまますぐに配信することができます。つまり、事前にパッケージされたこれらのストリーミング形式のバージョンを保存しておく必要がありません。

動的パッケージ化機能を利用するには、次の作業が必要となります。

* メザニン (ソース) ファイルを一連のアダプティブ ビットレート MP4 ファイルにエンコードする (エンコードの手順は、このチュートリアルで後ほど説明します)。  
* コンテンツ配信元となる " *ストリーミング エンドポイント* " のストリーミング ユニットを少なくとも 1 つ作成する。 以下の手順で、ストリーミング ユニットの数を変更する方法を示します。

ダイナミック パッケージを使用した場合、保存と課金の対象となるのは、単一のストレージ形式のファイルのみです。Media Services がクライアントからの要求に応じて適切な応答を構築して返します。

ストリーミング予約ユニットを作成したり、数を変更したりするには、以下の手順を実行します。

1. **[設定]** ウィンドウで **[ストリーミング エンドポイント]** をクリックします。
2. 既定のストリーミング エンドポイントをクリックします。

    **[DEFAULT STREAMING ENDPOINT DETAILS (既定のストリーミング エンドポイントの詳細)]** ウィンドウが表示されます。
3. ストリーミング ユニットの数を指定するには、 **[ストリーミング ユニット]** のスライダーを動かします。

    ![[ストリーミング ユニット]](./media/media-services-portal-vod-get-started/media-services-streaming-units.png)
4. **[保存]** をクリックして、変更を保存します。

   > [!NOTE]
   > 新しいユニットの割り当てが完了するまでに最大 20 分かかる場合があります。
   >
   >

## <a name="create-and-configure-a-visual-studio-project"></a>Visual Studio プロジェクトの作成と構成

1. Visual Studio 2013、Visual Studio 2012 か Visual Studio 2010 SP1 で、C# の新しいコンソール アプリケーションを作成します。 **[名前]**、**[場所]**、**[ソリューション名]** を入力し、**[OK]** をクリックします。
2. NuGet パッケージ [windowsazure.mediaservices.extensions](https://www.nuget.org/packages/windowsazure.mediaservices.extensions) を使用して **Azure Media Services .NET SDK Extensions** をインストールします。  Media Services .NET SDK Extensions は、コードを簡素化し、Media Services による開発を容易にする一連の拡張メソッドとヘルパー機能です。 このパッケージをインストールすると、 **Media Services .NET SDK** が一緒にインストールされるほか、必要な依存関係がすべて追加されます。

    NuGet を使用して参照を追加するには、ソリューション エクスプローラーで、プロジェクト名を右クリックし、**[NuGet パッケージの管理]** を選択します。 次に、**windowsazure.mediaservices.extensions** を検索し、**[インストール]** をクリックします。

3. System.Configuration アセンブリへの参照を追加します。 このアセンブリには、構成ファイル (App.config など) にアクセスするための **System.Configuration.ConfigurationManager** クラスが含まれています。

    参照を追加するには、ソリューション エクスプローラーで、プロジェクト名を右クリックし、**[追加]** > **[参照]** の順に選択し、検索ボックスに「configuration」と入力します。

4. App.config ファイルを開き (既定で追加されていない場合はファイルをプロジェクトに追加してください)、ファイルに *appSettings* セクションを追加します。 Azure Media Services のアカウント名とアカウント キーの値を設定します。次の例をご覧ください。 アカウント名とキーの情報を取得するには、[Azure Portal](https://portal.azure.com/) に移動して AMS アカウントを選択します。 次に、**[設定]** > **[キー]** の順に選択します。 [キーの管理] ウィンドウに、アカウント名、プライマリ キー、セカンダリ キーが表示されます。 アカウント名とプライマリ キーの値をコピーします。

        <configuration>
        ...
          <appSettings>
            <add key="MediaServicesAccountName" value="Media-Services-Account-Name" />
            <add key="MediaServicesAccountKey" value="Media-Services-Account-Key" />
          </appSettings>

        </configuration>
5. Program.cs ファイルの先頭にある既存の **using** ステートメントを次のコードで上書きします。

        using System;
        using System.Collections.Generic;
        using System.Linq;
        using System.Text;
        using System.Threading.Tasks;
        using System.Configuration;
        using System.Threading;
        using System.IO;
        using Microsoft.WindowsAzure.MediaServices.Client;
6. 新しいフォルダーをローカル ドライブの任意の場所に作成し、エンコードしてストリーミングするかプログレッシブ ダウンロードする .mp4 ファイルをコピーします。 この例では、"C:\VideoFiles" というパスを使用しています。

## <a name="connect-to-the-media-services-account"></a>Media Services アカウントへの接続

Media Services を .NET で使用するとき、Media Services に関連したプログラミング タスクの大半、たとえば、各種オブジェクト (資産、資産ファイル、ジョブ、アクセス ポリシー、ロケーターなど) の作成、更新、アクセス、削除の作業で、 **CloudMediaContext** クラスが必要となります。

既定の Program クラスを次のコードで上書きします。 このコードは、App.config ファイルから接続値を読み取り、 **CloudMediaContext** オブジェクトを作成して Media Services に接続する方法を示しています。 Media Services への接続の詳細については、「 [Media Services SDK for .NET を使用した Media Services への接続](http://msdn.microsoft.com/library/azure/jj129571.aspx)」をご覧ください。

メディア ファイルのある場所に合わせて、ファイル名とパスを更新してください。

**Main** 関数で呼び出すメソッドは、この後、定義していきます。

> [!NOTE]
> すべての関数の定義を追加するまでは、コンパイル エラーが発生します。

    class Program
    {
        // Read values from the App.config file.
        private static readonly string _mediaServicesAccountName =
            ConfigurationManager.AppSettings["MediaServicesAccountName"];
        private static readonly string _mediaServicesAccountKey =
            ConfigurationManager.AppSettings["MediaServicesAccountKey"];

        // Field for service context.
        private static CloudMediaContext _context = null;
        private static MediaServicesCredentials _cachedCredentials = null;

        static void Main(string[] args)
        {
            try
            {
                // Create and cache the Media Services credentials in a static class variable.
                _cachedCredentials = new MediaServicesCredentials(
                                _mediaServicesAccountName,
                                _mediaServicesAccountKey);
                // Used the chached credentials to create CloudMediaContext.
                _context = new CloudMediaContext(_cachedCredentials);

                // Add calls to methods defined in this section.
        // Make sure to update the file name and path to where you have your media file.
                IAsset inputAsset =
                    UploadFile(@"C:\VideoFiles\BigBuckBunny.mp4", AssetCreationOptions.None);

                IAsset encodedAsset =
                    EncodeToAdaptiveBitrateMP4s(inputAsset, AssetCreationOptions.None);

                PublishAssetGetURLs(encodedAsset);
            }
            catch (Exception exception)
            {
                // Parse the XML error message in the Media Services response and create a new
                // exception with its content.
                exception = MediaServicesExceptionParser.Parse(exception);

                Console.Error.WriteLine(exception.Message);
            }
            finally
            {
                Console.ReadLine();
            }
        }
    }

## <a name="create-a-new-asset-and-upload-a-video-file"></a>新しい資産の作成とビデオ ファイルのアップロード

Media Services で、デジタル ファイルを資産にアップロードし (取り込み) ます。 **Asset** エンティティには、ビデオ、オーディオ、画像、サムネイル コレクション、テキスト トラック、クローズド キャプション ファイル (各ファイルのメタデータを含む) を追加できます。ファイルをアップロードすると、クラウドにコンテンツが安全に保存され、処理したりストリーミングしたりできるようになります。 資産内のこれらのファイルを **資産ファイル**といいます。

以下に定義した **UploadFile** メソッドは、(.NET SDK Extensions で定義されている) **CreateFromFile** を呼び出します。 **CreateFromFile** によって、指定されたソース ファイルのアップロード先となる新しいアセットが作成されます。

**CreateFromFile** メソッドの **AssetCreationOptions** 引数には、次のいずれかの資産作成オプションを指定できます。

* **None** : 暗号化は使用されません。 これが既定値です。 このオプションを使用した場合、送信経路上とストレージ内のいずれにおいてもコンテンツが保護されないので注意してください。
  プログレッシブ ダウンロードを使用して MP4 を配信する場合はこのオプションを使用します。
* **StorageEncrypted** – Advanced Encryption Standard (AES)-256 ビット暗号化を使用してクリア コンテンツをローカルに暗号化し、それを Azure Storage にアップロードすると、コンテンツが保存時に暗号化された状態で格納されます。 StorageEncrypted で保護された資産は、エンコーディングの前に自動的に暗号化が解除され、暗号化されたファイル システムに配置されます。その後、必要に応じて再度暗号化を適用して、新しい出力資産として再びアップロードできます。 StorageEncrypted の主な目的は、高品質の入力メディア ファイルを強力な暗号化によって保護したうえでディスクに保存するというニーズに応えることです。
* **CommonEncryptionProtected** : 既に Common Encryption や PlayReady DRM で暗号化されて保護されているコンテンツ (PlayReady DRM で保護されたスムーズ ストリーミングなど) をアップロードする場合は、このオプションを使用します。
* **EnvelopeEncryptionProtected** : AES で暗号化された HLS をアップロードする場合はこのオプションを使用します。 この場合ファイルは、Transform Manager によってあらかじめエンコードされて暗号化されている必要があります。

**CreateFromFile** メソッドには、ファイルのアップロードの進行状況をレポートするためのコールバックも指定できます。

次の例では、資産のオプションに **None** を指定しています。

次のメソッドを Program クラスに追加します。

    static public IAsset UploadFile(string fileName, AssetCreationOptions options)
    {
        IAsset inputAsset = _context.Assets.CreateFromFile(
            fileName,
            options,
            (af, p) =>
            {
                Console.WriteLine("Uploading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Asset {0} created.", inputAsset.Id);

        return inputAsset;
    }


## <a name="encode-the-source-file-into-a-set-of-adaptive-bitrate-mp4-files"></a>一連のアダプティブ ビットレート MP4 ファイルにソース ファイルをエンコードする
Media Services に取り込んだ資産には、メディアのエンコード、再パッケージ化、透かしの追加などをクライアントへの配信前に適用できます。 高いパフォーマンスと可用性を確保するために、これらの作業は、複数のバックグラウンド ロール インスタンスに対してスケジューリングされて実行されます。 これらのアクティビティはジョブと呼ばれ、各ジョブは、資産ファイルの実際の作業を実行するアトミック タスクで構成されます。

冒頭で述べたように、Azure Media Services の代表的な用途の 1 つは、クライアントに対するアダプティブ ビットレート ストリーミング配信です。 Media Services では、HTTP ライブ ストリーミング (HLS)、Smooth Streaming、MPEG DASH のいずれかの形式に一連のアダプティブ ビットレート MP4 ファイルを動的にパッケージ化することができます。

動的パッケージ化機能を利用するには、次の作業が必要となります。

* mezzanine (ソース) ファイルを一連のアダプティブ ビットレート MP4 ファイルまたはアダプティブ ビットレート スムーズ ストリーミング ファイルにエンコードまたはトランスコードする。  
* コンテンツ配信元となるストリーミング エンドポイントのストリーミング ユニットを少なくとも 1 つ取得する。

以下のコードは、エンコーディング ジョブの送信方法を示したものです。 このジョブには、 **Media Encoder Standard**を使用して mezzanine ファイルを一連のアダプティブ ビットレート MP4 にトランスコードするよう指定するタスクが 1 つ存在します。 このコードは、ジョブを送信してその完了を待機します。

エンコード ジョブが完了したら、資産を発行し、MP4 ファイルのストリーミングまたはプログレッシブ ダウンロードを行うことができるようになります。

次のメソッドを Program クラスに追加します。

    static public IAsset EncodeToAdaptiveBitrateMP4s(IAsset asset, AssetCreationOptions options)
    {

        // Prepare a job with a single task to transcode the specified asset
        // into a multi-bitrate asset.

        IJob job = _context.Jobs.CreateWithSingleTask(
            "Media Encoder Standard",
            "H264 Multiple Bitrate 720p",
            asset,
            "Adaptive Bitrate MP4",
            options);

        Console.WriteLine("Submitting transcoding job...");


        // Submit the job and wait until it is completed.
        job.Submit();

        job = job.StartExecutionProgressTask(
            j =>
            {
                Console.WriteLine("Job state: {0}", j.State);
                Console.WriteLine("Job progress: {0:0.##}%", j.GetOverallProgress());
            },
            CancellationToken.None).Result;

        Console.WriteLine("Transcoding job finished.");

        IAsset outputAsset = job.OutputMediaAssets[0];

        return outputAsset;
    }

## <a name="publish-the-asset-and-get-urls-for-streaming-and-progressive-download"></a>資産を発行してストリーミング URL とプログレッシブ ダウンロード URL を取得する

資産をストリーミングまたはダウンロードするにはまず、ロケーターを作成して資産を「発行」する必要があります。 資産に含まれているファイルには、ロケーターを通じてアクセスできます。 Media Services では、2 種類のロケーターがサポートされています。OnDemandOrigin ロケーターはメディアのストリーミング (MPEG DASH、HLS、スムーズ ストリーミングなど) に、Access Signature (SAS) ロケーターはメディア ファイルのダウンロードに使用します (SAS ロケーターの詳細については、[こちら](http://southworks.com/blog/2015/05/27/reusing-azure-media-services-locators-to-avoid-facing-the-5-shared-access-policy-limitation/)のブログをご覧ください)。

### <a name="some-details-about-url-formats"></a>URL 形式の詳細

ロケーターを作成したら、対象ファイルのストリーミングやダウンロードに使用する URL を作成できます。 このチュートリアルのサンプルでは、適切なブラウザーに貼り付けることができる URL を出力します。 このセクションでは、さまざまな形式の簡単な例を示します。

#### <a name="a-streaming-url-for-mpeg-dash-has-the-following-format"></a>MPEG DASH のストリーミング URL の形式は次のとおりです。

{ストリーミング エンドポイント名-Media Services アカウント名}.streaming.mediaservices.windows.net/{ロケーター ID}/{ファイル名}.ism/Manifest**(format=mpd-time-csf)**

#### <a name="a-streaming-url-for-hls-has-the-following-format"></a>HLS のストリーミング URL の形式は次のとおりです。

{ストリーミング エンドポイント名-Media Services アカウント名}.streaming.mediaservices.windows.net/{ロケーター ID}/{ファイル名}.ism/Manifest**(format=m3u8-aapl)**

#### <a name="a-streaming-url-for-smooth-streaming-has-the-following-format"></a>スムーズ ストリーミングのストリーミング URL の形式は次のとおりです。

{ストリーミング エンドポイント名-Media Services アカウント名}.streaming.mediaservices.windows.net/{ロケーター ID}/{ファイル名}.ism/Manifest


#### <a name="a-sas-url-used-to-download-files-has-the-following-format"></a>ファイルをダウンロードするための SAS URL の形式は次のとおりです。

{BLOB コンテナー名}/{資産名}/{ファイル名}/{SAS 署名}

Media Services .NET SDK Extensions には、発行済みの資産の URL を正しい形式で取得できるヘルパー メソッドが用意されています。

次のコードでは、.NET SDK Extensions を使用してロケーターを作成し、ストリーミング URL と プログレッシブ ダウンロード URL を取得しています。 また、ローカル フォルダーにファイルをダウンロードする方法もこのコードで確認できます。

次のメソッドを Program クラスに追加します。

    static public void PublishAssetGetURLs(IAsset asset)
    {
        // Publish the output asset by creating an Origin locator for adaptive streaming,
        // and a SAS locator for progressive download.

        _context.Locators.Create(
            LocatorType.OnDemandOrigin,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));

        _context.Locators.Create(
            LocatorType.Sas,
            asset,
            AccessPermissions.Read,
            TimeSpan.FromDays(30));


        IEnumerable<IAssetFile> mp4AssetFiles = asset
                .AssetFiles
                .ToList()
                .Where(af => af.Name.EndsWith(".mp4", StringComparison.OrdinalIgnoreCase));

        // Get the Smooth Streaming, HLS and MPEG-DASH URLs for adaptive streaming,
        // and the Progressive Download URL.
        Uri smoothStreamingUri = asset.GetSmoothStreamingUri();
        Uri hlsUri = asset.GetHlsUri();
        Uri mpegDashUri = asset.GetMpegDashUri();

        // Get the URls for progressive download for each MP4 file that was generated as a result
        // of encoding.
        List<Uri> mp4ProgressiveDownloadUris = mp4AssetFiles.Select(af => af.GetSasUri()).ToList();


        // Display  the streaming URLs.
        Console.WriteLine("Use the following URLs for adaptive streaming: ");
        Console.WriteLine(smoothStreamingUri);
        Console.WriteLine(hlsUri);
        Console.WriteLine(mpegDashUri);
        Console.WriteLine();

        // Display the URLs for progressive download.
        Console.WriteLine("Use the following URLs for progressive download.");
        mp4ProgressiveDownloadUris.ForEach(uri => Console.WriteLine(uri + "\n"));
        Console.WriteLine();

        // Download the output asset to a local folder.
        string outputFolder = "job-output";
        if (!Directory.Exists(outputFolder))
        {
            Directory.CreateDirectory(outputFolder);
        }

        Console.WriteLine();
        Console.WriteLine("Downloading output asset files to a local folder...");
        asset.DownloadToFolder(
            outputFolder,
            (af, p) =>
            {
                Console.WriteLine("Downloading '{0}' - Progress: {1:0.##}%", af.Name, p.Progress);
            });

        Console.WriteLine("Output asset files available at '{0}'.", Path.GetFullPath(outputFolder));
    }

## <a name="test-by-playing-your-content"></a>コンテンツを再生することでテストする

前のセクションで定義したプログラムを実行すると、コンソール ウィンドウに次のような URL が表示されます。

アダプティブ ストリーミングの URL:

スムーズ ストリーミング

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest

HLS

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=m3u8-aapl)

MPEG DASH

    http://amstestaccount001.streaming.mediaservices.windows.net/ebf733c4-3e2e-4a68-b67b-cc5159d1d7f2/BigBuckBunny.ism/manifest(format=mpd-time-csf)

プログレッシブ ダウンロードの URL (オーディオとビデオ):

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_650kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_3400kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_2250kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1500kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_H264_1000kbps_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_96kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z

    https://storagetestaccount001.blob.core.windows.net/asset-38058602-a4b8-4b33-b9f0-6880dc1490ea/BigBuckBunny_AAC_und_ch2_56kbps.mp4?sv=2012-02-12&sr=c&si=166d5154-b801-410b-a226-ee2f8eac1929&sig=P2iNZJAvAWpp%2Bj9yV6TQjoz5DIIaj7ve8ARynmEM6Xk%3D&se=2015-02-14T01:13:05Z


ビデオをストリーミングするには、[Azure Media Services プレーヤー](http://amsplayer.azurewebsites.net/azuremediaplayer.html)の URL テキスト ボックスに URL を貼り付けます。

プログレッシブ ダウンロードをテストするには、ブラウザー (Internet Explorer、Chrome、Safari など) に URL を貼り付けます。

詳細については、次のトピックを参照してください。

- [既存のプレーヤーによるコンテンツの再生](media-services-playback-content-with-existing-players.md)
- [ビデオ プレーヤー アプリケーションの開発](media-services-develop-video-players.md)
- [DASH.js を使用した HTML5 アプリケーションへの MPEG-DASH アダプティブ ストリーミング ビデオの埋め込み](media-services-embed-mpeg-dash-in-html5.md)

## <a name="download-sample"></a>サンプルのダウンロード
このチュートリアルで作成したコードは、[このサンプル コード](https://azure.microsoft.com/documentation/samples/media-services-dotnet-on-demand-encoding-with-media-encoder-standard/)に含まれています。

## <a name="next-steps"></a>次のステップ 

[!INCLUDE [media-services-learning-paths-include](../../includes/media-services-learning-paths-include.md)]

## <a name="provide-feedback"></a>フィードバックの提供
[!INCLUDE [media-services-user-voice-include](../../includes/media-services-user-voice-include.md)]



<!-- Anchors. -->


<!-- URLs. -->
[Web Platform Installer]: http://go.microsoft.com/fwlink/?linkid=255386
[Portal]: http://manage.windowsazure.com/



<!--HONumber=Jan17_HO1-->


