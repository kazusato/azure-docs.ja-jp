---
title: Log Analytics の新しいログ検索についてよく寄せられる質問 | Microsoft Docs
description: この記事では、よく寄せられる新しいクエリ言語への Log Analytics のアップグレードに関する質問について説明します。
services: log-analytics
documentationcenter: ''
author: bwren
manager: carmonm
editor: ''
ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/19/2018
ms.author: bwren
ms.openlocfilehash: 6dfee26d7585c8ec295a1f0ea1bd0bc14a34cc5a
ms.sourcegitcommit: 1fbaa2ccda2fb826c74755d42a31835d9d30e05f
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/22/2018
ms.locfileid: "27993871"
---
# <a name="log-analytics-new-log-search-faq-and-known-issues"></a>Log Analytics の新しいログ検索についての FAQ と既知の問題

この記事では、[新しいクエリ言語への Log Analytics](log-analytics-log-search-upgrade.md) のアップグレードに関してよく寄せられる質問と既知の問題について説明します。  ワークスペースのアップグレードを決定する前に、この記事全体を確認してください。


## <a name="alerts"></a>アラート

### <a name="question-i-have-a-lot-of-alert-rules-do-i-need-to-create-them-again-in-the-new-language-after-i-upgrade"></a>質問: 警告ルールが多数あります。 アップグレード後、そのルールを新しい言語で作成し直す必要がありますか。  
いいえ。アップグレード中、警告ルールは新しい検索言語に自動変換されます。  

### <a name="question-i-have-alert-rules-with-webhook-and-runbook-actions-will-these-continue-to-work-when-i-upgrade"></a>質問: webhook アクションと Runbook アクションを含んだアラート ルールがあります。 これらはアップグレード後も使用できますか。

いいえ。webhook アクションと Runbook アクションに対するいくつかの変更に伴い、ペイロードの処理方法に変更が必要となる場合があります。 これらの変更は、各種の出力形式を標準化し、ペイロードのサイズを削減する目的で行われたものです。 これらの形式の詳細については、「[Log Analytics のアラート ルールへのアクションの追加](log-analytics-alerts-actions.md)」を参照してください。


## <a name="computer-groups"></a>コンピューター グループ

### <a name="question-im-getting-errors-when-trying-to-use-computer-groups--has-their-syntax-changed"></a>質問: コンピューター グループを使おうとするとエラーが発生します。  構文が変更されたのですか。
はい。コンピューター グループに使用する構文は、ワークスペースがアップグレードされたときに変更されました。  詳しくは、「[Log Analytics のログ検索におけるコンピューター グループ](log-analytics-computer-groups.md)」のページをご覧ください。


## <a name="dashboards"></a>ダッシュボード

### <a name="question-can-i-still-use-dashboards-in-an-upgraded-workspace"></a>質問: アップグレードされたワークスペースでこれまでどおりダッシュボードを使用できますか。
アップグレードに伴い、**マイ ダッシュボード**の廃止プロセスを開始しているところです。  ワークスペースがアップグレードされる前にダッシュボードに追加したタイルは引き続き使用できますが、タイルを編集したり、新しいダッシュボードを追加したりすることはできません。  これまでどおり、[ビュー デザイナー](log-analytics-view-designer.md)を使用したビューの作成および編集と、ダッシュボードの作成を Azure Portal で行えます。また、ビュー デザイナーの方が機能が豊富です。


## <a name="log-searches"></a>ログ検索

### <a name="question-i-have-saved-searches-outside-of-my-upgraded-workspace-can-i-convert-them-to-the-new-search-language-automatically"></a>質問: アップグレードされたワークスペース以外で検索を保存しました。 その検索を、新しい検索言語に自動変換できますか。
ログ検索ページで言語コンバーター ツールを使用して、1 つずつ変換できます。  ワークスペースをアップグレードしないで、複数の検索を自動的に変換することはできません。

### <a name="question-why-are-my-query-results-not-sorted"></a>質問: クエリの結果がソートされていないのはなぜですか。
既定では、新しいクエリ言語では結果の並べ替えは行われません。  [sort 演算子](https://go.microsoft.com/fwlink/?linkid=856079)を使用して、1 つまたは複数のプロパティで結果を並べ替えます。

### <a name="question-where-did-the-metrics-view-go-after-i-upgraded"></a>質問: アップグレード後、メトリック ビューはどこに移動されましたか。
メトリック ビューには、ログ検索からのパフォーマンス データがグラフィカルに表示されます。  アップグレード後は、このビューが使用できなくなります。  [レンダリング演算子](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/render-operator)を使用することで、クエリからの出力を時間グラフとして書式設定することができます。

### <a name="question-where-did-minify-go-after-i-upgraded"></a>質問: アップグレードした後、縮小はどこに移動されましたか?
縮小は、検索結果の概要を把握する機能です。  アップグレードした後、縮小オプションは、ログ検索ポータルに表示されなくなります。  新しい検索言語では、[reduce](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/reduce-operator) または [autocluster_v2](https://docs.loganalytics.io/docs/Language-Reference/Tabular-operators/evaluate-operator/autocluster) を使用して類似した機能を使用できます。 

    Event
    | where TimeGenerated > ago(10h)
    | reduce by RenderedDescription

    Event
    | where TimeGenerated > ago(10h)
    | evaluate autocluster_v2()



## <a name="log-search-api"></a>Log Search API

### <a name="question-does-the-log-search-api-get-updated-after-i-upgrade"></a>質問: アップグレード後に Log Search API は更新されますか。
従来の [Log Search API](log-analytics-log-search-api.md) は、ワークスペースをアップグレードすると動作しなくなります。  新しい API の詳細については、「[Azure Log Analytics REST API](https://dev.loganalytics.io/)」を参照してください。


## <a name="portals"></a>ポータル

### <a name="question-should-i-use-the-new-advanced-analytics-portal-or-keep-using-the-log-search-portal"></a>質問: 新しい Advanced Analytics ポータルを使用する必要はありますか。またはログ検索ポータルを使用し続ける必要はありますか。
「[Azure Log Analytics でログ クエリを作成および編集するためのポータル](log-analytics-log-search-portals.md)」で、2 つのポータルを比較できます。  利点はそれぞれに異なるため、要件に最適なものを選択できます。  Advanced Analytics ポータルでクエリを記述し、ビュー デザイナーなどの他の場所に貼り付ける点は共通です。  実行にあたっては、[考慮すべき問題](log-analytics-log-search-portals.md#advanced-analytics-portal)をお読みください。


### <a name="question--after-upgrade-i-get-an-error-trying-to-run-queries-and-am-also-seeing-errors-in-my-views"></a>質問: アップグレード後、クエリを実行しようとするとエラーが発生します。また、ビューにもエラーが表示されます。

アップグレード後、Log Analytics クエリを実行するには、ブラウザーが次のアドレスにアクセスできる必要があります。  ブラウザーがファイアウォールを介して Azure Portal にアクセスしている場合は、これらのアドレスへのアクセスを有効にする必要があります。

| Uri | IP | ポート |
|:---|:---|:---|
| portal.loganalytics.io | 動的 | 80,443 |
| api.loganalytics.io    | 動的 | 80,443 |
| docs.loganalytics.io   | 動的 | 80,443 |



## <a name="power-bi"></a>Power BI

### <a name="question-does-anything-change-with-powerbi-integration"></a>質問: PowerBI 統合に変更はありますか。
はい。  ワークスペースのアップグレードが完了すると、Power BI への Log Analytics データのエクスポート プロセスは動作しなくなります。  また、アップグレード前に作成した既存のスケジュールすべてが無効になります。  

アップグレード後、Azure Log Analytics では Application Insights と同じプラットフォームが使用されるため、[Application Insights クエリを Power BI にエクスポートするプロセス](../application-insights/app-insights-export-power-bi.md#export-analytics-queries)と同じプロセスを使用して、Power BI に Log Analytics クエリをエクスポートします。  Power BI へのエクスポートでは、API エンドポイントを直接呼び出すようになりました。 これにより、最大 500,000 行または 64,000,000 バイトのデータの取得、時間がかかるクエリのエクスポート、およびクエリのタイムアウトのカスタマイズ (既定のタイムアウトは 3 分間でタイムアウトの最大値は 10 分) を実行できます。

## <a name="powershell-cmdlets"></a>PowerShell コマンドレット

### <a name="question-does-the-log-search-powershell-cmdlet-get-updated-after-i-upgrade"></a>質問: アップグレード後にログ検索の PowerShell コマンドレットは更新されますか。
[Get-AzureRmOperationalInsightsSearchResults](https://docs.microsoft.com/powershell/module/azurerm.operationalinsights/Get-AzureRmOperationalInsightsSearchResults) は、すべてのワークスペースのアップグレードが完了した時点で、使用されなくなる予定です。  アップグレードされたワークスペースでログ検索を実行するには、[Invoke-LogAnalyticsQuery cmdlet](https://dev.loganalytics.io/documentation/Tools/PowerShell-Cmdlets) を使用してください。




## <a name="resource-manager-templates"></a>Resource Manager テンプレート

### <a name="question-can-i-create-an-upgraded-workspace-with-a-resource-manager-template"></a>質問: アップグレードされたワークスペースを、リソース マネージャー テンプレートを使用して作成できますか。
はい。  API のプレビュー バージョン 2017-03-15 を使用し、次の例のように、テンプレートに**機能**セクションを含める必要があります。

    "resources": [
        {
            "type": "Microsoft.OperationalInsights/workspaces",
            "apiVersion": "2017-03-15-preview",
            "name": "[parameters('workspaceName')]",
            "location": "[parameters('workspaceRegion')]",
            "properties": {
                "sku": {
                    "name": "Free"
                },
                "features": {
                    "legacy": 0,
                    "searchVersion": 1
                }
            }
        }
    ],



## <a name="solutions"></a>解決方法

### <a name="question-will-my-solutions-continue-to-work"></a>質問: ソリューションは引き続き機能しますか。
すべてのソリューションは、アップグレードされたワークスペースでこれまでどおり機能しますが、新しいクエリ言語に変換すると、パフォーマンスが向上します。  既知の問題の中には、このセクションで説明されている解決方法が存在するものがあります。

### <a name="known-issue-perspectives-in-application-insights-connector"></a>既知の問題: Application Insights Connector のパースペクティブ
[Application Insights Connector ソリューション](log-analytics-app-insights-connector.md)のパースペクティブは、Application Insights コネクタ ソリューションではサポートされなくなりました。  ビュー デザイナーを使用して、Application Insights データのカスタム ビューを作成できます。

### <a name="known-issue-backup-solution"></a>既知の問題: Microsoft Azure Backup ソリューション
ワークスペースをアップグレードする前にBackup ソリューションをインストールした場合、Backup ソリューションでデータが収集されない可能性があります。 ソリューションをアンインストールした後、最新バージョンをインストールしてください。  新しいバージョンのソリューションは、従来の Backup コンテナーをサポートしないため、引き続きソリューションを使用するには、Recovery Services コンテナーもアップグレードする必要があります。

## <a name="upgrade-process"></a>アップグレード プロセス

### <a name="question-i-have-several-workspaces-can-i-upgrade-all-workspaces-at-the-same-time"></a>質問: ワークスペースが複数あります。 すべてのワークスペースを同時にアップグレードできますか。  

いいえ。  アップグレードはワークスペースごとに 1 つずつ適用されます。 現時点では、多数のワークスペースを一度にアップグレードすることはできません。 ワークスペースをアップグレードすると、そのワークスペースの他のユーザーも影響を受けることに注意してください。  

### <a name="question-will-existing-log-data-collected-in-my-workspace-be-modified-if-i-upgrade"></a>質問: アップグレードすると、ワークスペースで収集された既存のログ データは変更されますか。  

いいえ。 ワークスペース検索で使用できるログ データは、アップグレードの影響を受けません。 保存した検索、警告、およびビューは、新しい検索言語に自動的に変換されます。  

### <a name="question-what-happens-if-i-dont-upgrade-my-workspace"></a>質問: ワークスペースをアップグレードしないとどうなりますか。  
従来のログ検索は、数か月後に廃止される予定です。 その時点でアップグレードされていないワークスペースは、自動的にアップグレードされます。

### <a name="question-can-i-revert-back-after-i-upgrade"></a>質問: アップグレード後、元に戻すことはできますか。
一般公開の前は、ワークスペースをアップグレード後に、元に戻すことができました。  現在では新しい言語が一般公開となり、レガシ プラットフォームの廃止が開始されているため、この機能は削除されています。


## <a name="views"></a>ビュー

### <a name="question-how-do-i-create-a-new-view-with-view-designer"></a>質問: ビュー デザイナーで新しいビューを作成するにはどうすればいいですか。
アップグレードの前に、OMS ポータル内のメイン ダッシュボードのタイルから、ビュー デザイナーで新しいビューを作成できます。  ワークスペースがアップグレードされると、このタイルは削除されます。  左側のメニュー ボタンで緑色の + ボタンをクリックすると、OMS ポータルのビュー デザイナーで新しいビューを作成できます。  ビュー デザイナー タイルをクリックして、引き続き Azure Portal で新しいビューを作成します。


## <a name="next-steps"></a>次の手順

- [新しい Log Analytics クエリ言語へのワークスペースのアップグレード](log-analytics-log-search-upgrade.md)について確認します。
