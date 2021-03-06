---
title: 什麼是 Azure 應用程式設定？ | Microsoft Docs
description: Azure 應用程式設定服務的概觀。
services: azure-app-configuration
documentationcenter: ''
author: yegu-ms
manager: balans
editor: ''
ms.service: azure-app-configuration
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: overview
ms.date: 02/24/2019
ms.author: yegu
ms.openlocfilehash: 8c2c31b4d87ac2db123c0cae63679e3773734b4f
ms.sourcegitcommit: 12d67f9e4956bb30e7ca55209dd15d51a692d4f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/20/2019
ms.locfileid: "58226278"
---
# <a name="what-is-azure-app-configuration"></a>什麼是 Azure 應用程式設定？

Azure 應用程式設定提供集中管理應用程式設定的服務。 最新的程式 (特別是在雲端中執行的程式) 通常會有許多分散性質的元件。 將組態設定分散到這些元件，可能會導致應用程式部署期間發生難以排解的錯誤。 使用應用程式設定儲存應用程式的所有設定，並在同一個地方保護其存取權。

在預覽期間可免費使用應用程式設定。 如果您想要試用，請[註冊](https://aka.ms/azconfig/register)預覽版。

## <a name="why-use-app-configuration"></a>為什麼使用應用程式設定？

雲端式應用程式通常會在多個區域中的多個虛擬機器或容器上執行，並使用多個外部服務。 建立這類穩固且可調整的分散式應用程式是一項挑戰。 

為協助開發人員在建置應用程式時，能處理不斷提高的複雜度，現在有各式各樣的程式設計方法可使用。 例如，12-factor 應用程式會描述許多通過完整測試的架構模式和最佳做法，以搭配雲端應用程式使用。 本指南中的其中一個重要建議就是區隔組態與程式碼。 在此情況下，應用程式的組態設定應該保留在其可執行檔的外部，並從其執行階段環境或外部來源讀入。

任何應用程式都可使用應用程式設定，下列範例是可從中受益的應用程類型：

* 以一個或多個地理位置上所部署 Azure Kubernetes Service、Azure Service Fabric 或其他容器化應用程式為基礎的微服務
* 無伺服器應用程式，包括 Azure Functions 或其他由事件驅動的無狀態計算應用程式
* 持續部署管線

應用程式設定具有下列優點︰

* 能在幾分鐘內完成設定的完全受控服務
* 彈性的索引鍵表示法和對應
* 使用標籤進行標記
* 根據時間點重新執行設定
* 在自行定義的維度上比較兩個設定集
* 已透過 Azure 管理的身分識別增強安全性
* 完整的資料加密 (待用或傳輸中加密)
* 與熱門架構原生整合

應用程式設定可補充用來儲存應用程式祕密的 [Azure Key Vault](https://azure.microsoft.com/services/key-vault/) 不足之處。 應用程式設定可讓您更輕鬆地實作下列案例：

* 集中管理和發佈不同環境與地理位置的階層式設定資料
* 動態變更設定，而不需要重新部署或重新啟動應用程式
* 功能管理

## <a name="use-app-configuration"></a>使用應用程式設定

您可以透過 Microsoft 提供的用戶端程式庫，將應用程式設定存放區新增至您的應用程式，這是最簡單的方式。 以下是適用於不同程式設計語言和架構的最佳方法。

| 程式設計語言和架構 | 如何連接 |
|---|---|
| .NET Core 和 ASP.NET Core | 適用於 .NET Core 的應用程式設定提供者 |
| .NET 和 ASP.NET | 適用於 .NET 的應用程式設定建立器 |
| Java Spring | 適用於 Spring Cloud 的應用程式設定用戶端 |
| 其他 | 應用程式設定 REST API |

## <a name="next-steps"></a>後續步驟

* [快速入門：建立 ASP.NET Web 應用程式](quickstart-aspnet-core-app.md) 
