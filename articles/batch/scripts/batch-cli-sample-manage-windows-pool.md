---
title: Azure CLI のサンプル スクリプト - Batch での Windows プール | Microsoft Docs
description: Azure CLI のサンプル スクリプト - Batch での Windows プールの作成と管理
services: batch
documentationcenter: ''
author: dlepow
manager: jeconnoc
editor: ''
ms.assetid: ''
ms.service: batch
ms.devlang: azurecli
ms.topic: sample
ms.tgt_pltfrm: multiple
ms.workload: na
ms.date: 01/29/2018
ms.author: danlep
ms.openlocfilehash: 0d9c757925b26e9f46afbd075184f853efa25dc3
ms.sourcegitcommit: 8c3267c34fc46c681ea476fee87f5fb0bf858f9e
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/09/2018
ms.locfileid: "29843059"
---
# <a name="cli-example-create-and-manage-a-windows-pool-in-azure-batch"></a>CLI の例: Azure Batch での Windows プールの作成と管理

このスクリプトでは、Azure Batch で Windows コンピューティング ノードのプールを作成して管理するために、Azure CLI で使用できるコマンドの一部を示します。 Windows プールは、クラウド サービスの構成と仮想マシンの構成という 2 つの方法で構成できます。 この例では、クラウド サービスの構成を使用して Windows プールを作成する方法を示します。

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

CLI をローカルにインストールして使用する場合、この記事では、Azure CLI バージョン 2.0.20 以降を実行していることが要件です。 バージョンを確認するには、`az --version` を実行します。 インストールまたはアップグレードする必要がある場合は、「[Azure CLI 2.0 のインストール](/cli/azure/install-azure-cli)」を参照してください。 

## <a name="example-script"></a>サンプル スクリプト

[!code-azurecli-interactive[main](../../../cli_scripts/batch/manage-pool/manage-pool-windows.sh "Manage Windows Cloud Services Pool")]

## <a name="clean-up-deployment"></a>デプロイのクリーンアップ

次のコマンドを実行して、リソース グループと、それに関連付けられているすべてのリソースを削除します。

```azurecli-interactive
az group delete --name myResourceGroup
```

## <a name="script-explanation"></a>スクリプトの説明

このスクリプトでは、次のコマンドを使用します。 表内の各コマンドは、それぞれのドキュメントにリンクされています。

| コマンド | メモ |
|---|---|
| [az group create](/cli/azure/group#az_group_create) | すべてのリソースを格納するリソース グループを作成します。 |
| [az batch account create](/cli/azure/batch/account#az_batch_account_create) | Batch アカウントを作成します。 |
| [az batch account login](https://docs.microsoft.com/cli/azure/batch/account#az_batch_account_login) | さらに CLI と対話できるように、指定された Batch アカウントを認証します。 |
| [az batch pool create](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_create) | コンピューティング ノードのプールを作成します。  |
| [az batch pool set](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_set) | プールのプロパティを更新します。  |
| [az batch pool autoscale enable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_enable) | プールで自動スケーリングを有効にし、式を適用します。  |
| [az batch pool show](https://docs.microsoft.com/cli/azure/batch/pool#az_batch_pool_show) | プールのプロパティを表示します。  |
| [az batch pool autoscale disable](https://docs.microsoft.com/cli/azure/batch/pool/autoscale#az_batch_pool_autoscale_disable) | プールで自動スケーリングを無効にします。 |
| [az group delete](/cli/azure/group#az_group_delete) | 入れ子になったリソースすべてを含むリソース グループを削除します。 |


## <a name="next-steps"></a>次の手順

Azure CLI の詳細については、[Azure CLI のドキュメント](https://docs.microsoft.com/cli/azure)のページをご覧ください。
