---
title: チュートリアル - Azure CDN キャッシュ規則の設定 | Microsoft Docs
description: このチュートリアルでは、Azure CDN のグローバルおよびカスタム キャッシュ規則を設定します。
services: cdn
documentationcenter: ''
author: dksimpson
manager: akucer
editor: ''
ms.service: cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: tutorial
ms.date: 04/20/2018
ms.author: v-deasim
ms.custom: mvc
ms.openlocfilehash: 4f586c47552c9c2bfa807120f99eb6cf170b5788
ms.sourcegitcommit: e2adef58c03b0a780173df2d988907b5cb809c82
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/28/2018
---
# <a name="tutorial-set-azure-cdn-caching-rules"></a>チュートリアル: Azure CDN キャッシュ規則の設定

> [!NOTE] 
> Azure CDN キャッシュ規則は、**Azure CDN Standard from Verizon** および **Azure CDN Standard from Akamai** でのみ使用できます。 **Azure CDN Premium from Verizon** の場合は、同様の機能のために、**管理**ポータルの [Azure CDN ルール エンジン](cdn-rules-engine.md)を使用します。
 

このチュートリアルでは、Azure Content Delivery Network (CDN) のキャッシュ規則を使って、グローバルと、URL パスやファイル拡張子などのカスタム条件の両方で、既定のキャッシュ期限切れ動作を設定または変更する方法について説明します。 Azure CDN では、2 種類のキャッシュ規則が提供されます。
- グローバル キャッシュ規則: プロファイルのエンドポイントごとに 1 つのグローバル キャッシュ規則を設定することができ、エンドポイントに対するすべての要求に適用されます。 HTTP キャッシュ ディレクティブ ヘッダーが設定されている場合、グローバル キャッシュ規則はそれより優先されます。

- カスタム キャッシュ規則: プロファイルのエンドポイントごとに、1 つ以上のカスタム キャッシュ規則を設定できます。 カスタム キャッシュ規則は、特定のパスおよびファイル拡張子と一致し、順番に処理され、グローバル キャッシュ規則が設定されている場合はそれより優先されます。 

このチュートリアルで学習する内容は次のとおりです。
> [!div class="checklist"]
> - キャッシュ規則のページを開きます。
> - グローバル キャッシュ規則を作成します。
> - カスタム キャッシュ規則を作成します。

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>前提条件

このチュートリアルの手順を完了するには、最初に CDN プロファイルと少なくとも 1 つの CDN エンドポイントを作成する必要があります。 詳細については、「[クイック スタート: Azure CDN プロファイルとエンドポイントの作成](cdn-create-new-endpoint.md)」を参照してください。

## <a name="open-the-azure-cdn-caching-rules-page"></a>Azure CDN のキャッシュ規則のページを開く

1. [Azure Portal](https://portal.azure.com) で CDN プロファイルを選択し、エンドポイントを選択します。

2. 左側のウィンドウの [設定] で、**[キャッシュ規則]** を選択します。

   ![CDN の [キャッシュ規則] ボタン](./media/cdn-caching-rules/cdn-caching-rules-btn.png)

   **[キャッシュ規則]** ページが表示されます。

   ![CDN の [キャッシュ規則] ページ](./media/cdn-caching-rules/cdn-caching-rules-page.png)


## <a name="set-global-caching-rules"></a>グローバル キャッシュ規則を設定する

グローバル キャッシュ規則を次のように作成します。

1. **[グローバル キャッシュ ルール]** で、**[クエリ文字列のキャッシュ動作]** を **[クエリ文字列を無視]** に設定します。

2. **[キャッシュ動作]** を **[存在しない場合に設定]** に設定します。
       
3. **[キャッシュの有効期間]** で、**[日]** フィールドに「10」と入力します。

    グローバル キャッシュ規則は、エンドポイントに対するすべての要求に適用されます。 この規則は、元の要求にキャッシュ ディレクティブ ヘッダー (`Cache-Control`または`Expires`) が存在する場合はそれに従い、指定されていない場合はキャッシュを 10 日に設定します。 

    ![グローバル キャッシュ ルール](./media/cdn-caching-rules/cdn-global-caching-rules.png)

## <a name="set-custom-caching-rules"></a>カスタム キャッシュ規則を設定する

カスタム キャッシュ規則を次のように作成します。

1. **[カスタム キャッシュ ルール]** で、**[一致条件]** を **[パス]** に、**[一致する値]** を「`/images/*.jpg`」に設定します。
    
2. **[キャッシュ動作]** を **[上書き]** に設定し、**[日]** フィールドに「30」と入力します。
       
    このカスタム キャッシュ規則は、エンドポイントの `/images` フォルダーにあるすべての `.jpg` イメージ ファイルについて、キャッシュの有効期間を 30 日に設定します。 要求元のサーバーによって送信された `Cache-Control` または `Expires` HTTP ヘッダーより、この規則の方が優先されます。

    ![カスタム キャッシュ ルール](./media/cdn-caching-rules/cdn-custom-caching-rules.png)

    
## <a name="clean-up-resources"></a>リソースのクリーンアップ

上記の手順で、キャッシュ規則を作成しました。 これらのキャッシュ規則を使用する必要がなくなった場合は、次の手順を実行して規則を削除できます。
 
1. CDN プロファイルを選択し、削除するキャッシュ規則があるエンドポイントを選択します。

2. 左側のウィンドウの [設定] で、**[キャッシュ規則]** を選択します。

3. **[グローバル キャッシュ ルール]** で、**[キャッシュ動作]** を **[未設定]** に設定します。
 
4. **[Custom caching rules (カスタム キャッシュ規則)]** で、削除する規則の横にあるチェック ボックスをオンにします。

5. **[削除]** を選択します。

6. ページの最上部から **[保存]** を選択します。


## <a name="next-steps"></a>次の手順

このチュートリアルで学習した内容は次のとおりです。

> [!div class="checklist"]
> - キャッシュ規則のページを開きます。
> - グローバル キャッシュ規則を作成します。
> - カスタム キャッシュ規則を作成します。

追加のキャッシュ規則の設定を構成する方法については、次の記事に進みます。

> [!div class="nextstepaction"]
> [キャッシュ規則で Azure CDN キャッシュの動作を制御する](cdn-caching-rules.md)



