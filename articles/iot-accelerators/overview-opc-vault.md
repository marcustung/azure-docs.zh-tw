---
title: 什麼是 Azure IoT OPC UA 憑證管理 | Microsoft Docs
description: OPC 保存庫概觀
author: dominicbetts
ms.author: dobett
ms.date: 11/26/2018
ms.topic: overview
ms.service: iot-industrialiot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: ba3313d927ec4317d6c051f6058d75a415b92288
ms.sourcegitcommit: 563f8240f045620b13f9a9a3ebfe0ff10d6787a2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/01/2019
ms.locfileid: "58759732"
---
# <a name="what-is-azure-iot-open-platform-communications-opc-ua-certificate-management"></a>什麼是 Azure IoT 開放平台通訊 (OPC) UA 憑證管理？

Azure IoT OPC UA 憑證管理也稱為 OPC 保存庫，這是一項微服務，可以設定、註冊和管理雲端中的 OPC UA 伺服器和用戶端應用程式的憑證生命週期。 本文將說明 OPC 保存庫的簡單使用案例。

## <a name="certificate-management"></a>憑證管理

例如，某家製造公司需要其 OPC UA 伺服器電腦連線至其新建置的用戶端應用程式。 當製造商初次存取伺服器電腦時，OPC UA 伺服器應用程式會立即顯示錯誤訊息，以指出用戶端應用程式並不安全。 這是 OPC UA 伺服器電腦的內建機制，旨在避免任何未經授權的應用程式存取，這可以防止工廠遭到惡毒駭客攻擊。

## <a name="application-security-management"></a>應用程式安全性管理
安全性專業人員可使用 OPC 保存庫微服務輕鬆地讓 OPC UA 伺服器與任何用戶端應用程式進行通訊，因為 OPC 保存庫具有憑證登錄、儲存體和生命週期管理的所有功能。 OPC UA 伺服器現在已安全地連線，可與新建置的用戶端應用程式進行通訊

## <a name="the-complete-opc-vault-architecture"></a>完整的 OPC 保存庫架構
下圖顯示完整的 OPC 保存庫架構。

![OPC 保存庫架構](media/overview-opc-vault-architecture/opc-vault.png)