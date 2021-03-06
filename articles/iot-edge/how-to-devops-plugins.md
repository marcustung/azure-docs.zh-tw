---
title: 搭配 Jenkins 外掛程式啟用 CI/CD - Azure IoT Edge | Microsoft Docs
description: 適用於 Jenkins 的 Azure IoT Edge 擴充可讓您將 IoT Edge 開發和部署工作整合到現有的 DevOps 解決方案中。
author: kgremban
manager: philmea
ms.author: kgremban
ms.date: 01/22/2019
ms.topic: conceptual
ms.service: iot-edge
services: iot-edge
ms.openlocfilehash: 173e6ff91acd2ad28d7203b2b5db65e0ee0ecc43
ms.sourcegitcommit: 97d0dfb25ac23d07179b804719a454f25d1f0d46
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2019
ms.locfileid: "54910821"
---
# <a name="integrate-azure-iot-edge-with-jenkins-pipelines"></a>整合 Azure IoT Edge 與 Jenkins 管線

Azure IoT Edge 內建針對 Azure DevOps 和 Azure DevOps Projects 的支援，但也提供將 DevOps 功能擴充到 Jenkins 的外掛程式。 [Jenkins](https://jenkins.io/) \(英文\) 是開放原始碼的自動化伺服器，其使用外掛程式來支援許多類型的開發和部署專案，包括 IoT Edge。 

持續整合與持續部署是適用於 Jenkins 的 Azure IoT Edge 外掛程式的重點。 您可以建立建置和推送管線，以建置模組並將其容器映像推送到您的容器登錄。 然後建立發行管線，以將該模組部署到您的 IoT Edge 裝置。 

在您開始使用適用於 Jenkins 的 IoT Edge 外掛程式之前，您需要 Azure 中的 IoT 中樞，以及容器登錄來保存您的容器映像。 使用 Azure 服務主體來將 IoT 中樞的參與者權限授與 Jenkins，使該外掛程式可以為您的 IoT Edge 裝置建立部署。 

如果您已準備好開始進行，請參閱[適用於 Jenkins 的 Azure IoT Edge 外掛程式](https://plugins.jenkins.io/azure-iot-edge) \(英文\) 安裝和使用詳細資料。