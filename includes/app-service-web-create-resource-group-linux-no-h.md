---
title: インクルード ファイル
description: インクルード ファイル
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: 56fa96810b9e47e817c64ecc1a0df4e6a0b3db93
ms.sourcegitcommit: 8aab1aab0135fad24987a311b42a1c25a839e9f3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
ms.locfileid: "29953849"
---
[!INCLUDE [resource group intro text](resource-group.md)]

Cloud Shell で [`az group create`](/cli/azure/group?view=azure-cli-latest#az_group_create) コマンドを使用して、リソース グループを作成します。 次の例では、*myResourceGroup* という名前のリソース グループを "*西ヨーロッパ*" の場所に作成します。 **Standard** レベルの Linux 上の App Service がサポートされているすべての場所を表示するには、[`az appservice list-locations --sku S1 --linux-workers-enabled`](/cli/azure/appservice?view=azure-cli-latest#az_appservice_list_locations) コマンドを実行します。

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

通常は、現在地付近の地域にリソース グループおよびリソースを作成します。 

コマンドが完了すると、リソース グループのプロパティが JSON 出力に表示されます。