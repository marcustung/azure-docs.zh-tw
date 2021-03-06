---
title: 包含檔案
description: 包含檔案
services: app-service
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 02/02/2018
ms.author: cephalin
ms.custom: include file
ms.openlocfilehash: b59b45de04ebb717dfe55eb17c9dbd92f7523976
ms.sourcegitcommit: e51e940e1a0d4f6c3439ebe6674a7d0e92cdc152
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/08/2019
ms.locfileid: "55896466"
---
[!INCLUDE [resource group intro text](resource-group.md)]

在 Cloud Shell 中，使用 [`az group create`](/cli/azure/group?view=azure-cli-latest) 命令來建立資源群組。 下列範例會在「西歐」位置建立名為 myResourceGroup 的資源群組。 若要查看**免類**層中 App Service 的所有支援位置，請執行 [`az appservice list-locations --sku FREE`](/cli/azure/appservice?view=azure-cli-latest) 命令。

```azurecli-interactive
az group create --name myResourceGroup --location "West Europe"
```

您通常會在附近的區域中建立資源群組和資源。 

當命令完成時，JSON 輸出會顯示資源群組屬性。