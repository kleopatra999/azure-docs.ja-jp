---
title: "Azure AD アプリケーション プロキシ コネクタの使用 | Microsoft Docs"
description: "Azure AD アプリケーション プロキシにおけるコネクタのグループの作成と管理の方法について説明します。"
services: active-directory
documentationcenter: 
author: kgremban
manager: femila
editor: 
ms.assetid: 5404372d-3092-4054-aeee-26afb1399f33
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/09/2016
ms.author: kgremban
translationtype: Human Translation
ms.sourcegitcommit: 2ea002938d69ad34aff421fa0eb753e449724a8f
ms.openlocfilehash: ba87b73be45cb0893f418453bc0efb3293772c95


---
# <a name="publish-applications-on-separate-networks-and-locations-using-connector-groups---public-preview"></a>コネクタ グループを使用して別のネットワークや場所にアプリケーションを発行する - パブリック プレビュー
> [!div class="op_single_selector"]
> * [Azure Portal](active-directory-application-proxy-connectors-azure-portal.md)
> * [Azure クラシック ポータル](active-directory-application-proxy-connectors.md)
> 
> 

コネクタ グループは、次のようなさまざまなシナリオで役立ちます。

* 複数のデータセンターが相互接続されたサイト。 この場合、データセンター間リンクはコストがかかり、低速であるため、できるだけ多くのトラフィックをデータセンター内に留める必要があります。 各データセンターにコネクタをデプロイして、データセンター内にあるアプリケーションにのみサービスを提供できます。 この方法で、データセンター間のリンクを最小限に抑えて、ユーザーに完全に透過的なエクスペリエンスを提供できます。
* メインの企業ネットワークに属していない分離されたネットワークにインストールされているアプリケーションの管理。 コネクタ グループを使用して、分離されたネットワークに専用のコネクタをインストールし、そのネットワークに対してアプリケーションの分離も実現できます。
* クラウド アクセスを実現するためにアプリケーションが IaaS 上にインストールされている場合。コネクタ グループは、すべてのアプリケーションへのアクセスをセキュリティで保護する共通サービスを提供します。 コネクタ グループによって、企業ネットワークに対する追加の依存関係が生じることも、アプリケーション エクスペリエンスが分断されることもありません。 各クラウド データセンターにコネクタをインストールし、このネットワーク上にあるアプリケーションにのみサービスを提供できます。 複数のコネクタをインストールして、高可用性を実現できます。
* マルチ フォレスト環境のサポート。フォレストごとに専用のコネクタをデプロイし、特定のアプリケーションにサービスを提供するよう設定できます。
* 障害復旧サイトでコネクタ グループを使用してフェールオーバーを検出したり、メイン サイトのバックアップとして使用したりできます。
* コネクタ グループを使用して、シングル テナントから複数の企業にサービスを提供できます。

## <a name="prerequisite-create-your-connectors"></a>前提条件: コネクタの作成
コネクタをグループ化するには、 [複数のコネクタがインストールされている](active-directory-application-proxy-enable.md)ことを確認する必要があります。 新しいコネクタをインストールすると、そのコネクタは自動的に **Default** コネクタ グループに追加されます。

## <a name="step-1-create-connector-groups"></a>手順 1: コネクタ グループを作成する
コネクタ グループは必要な数だけ作成できます。 コネクタ グループの作成は、 [Azure Portal](https://portal.azure.com) で実行します。

1. **[Azure Active Directory]** を選択して、ディレクトリの管理ダッシュボードに移動します。 管理ダッシュボードで、**[Enterprise applications (エンタープライズ アプリケーション)]** > **[アプリケーション プロキシ]** の順に選択します。
2. **[コネクタ グループ]** ボタンをクリックします。 [New Connector Group (新しいコネクタ グループ)] ブレードが表示されます。
3. 新しいコネクタ グループに名前を付け、ドロップダウン メニューを使用して、このグループに含めるコネクタを選択します。
4. コネクタ グループが完成したら、 **[保存]** をクリックします。

## <a name="step-2-assign-applications-to-your-connector-groups"></a>手順 2: コネクタ グループにアプリケーションを割り当てる
最後の手順は、アプリケーションにサービスを提供するコネクタ グループへの各アプリケーションの設定です。

1. ディレクトリの管理ダッシュボードで、**[Enterprise applications (エンタープライズ アプリケーション)]** > **[すべてのアプリケーション]**、コネクタ グループに割り当てるアプリケーション、**[アプリケーション プロキシ]** の順に選択します。
2. **[コネクタ グループ]**のドロップダウン メニューで、アプリケーションで使用するグループを選択します。
3. **[保存]** をクリックして変更を適用します。

## <a name="see-also"></a>関連項目
* [アプリケーション プロキシを有効にする](active-directory-application-proxy-enable.md)
* [シングル サインオンを有効にする](active-directory-application-proxy-sso-using-kcd.md)
* [条件付きアクセスを有効にする](active-directory-application-proxy-conditional-access.md)
* [アプリケーション プロキシで発生した問題のトラブルシューティングを行う](active-directory-application-proxy-troubleshoot.md)

最新のニュースと更新情報については、 [アプリケーション プロキシに関するブログ](http://blogs.technet.com/b/applicationproxyblog/)




<!--HONumber=Dec16_HO5-->


