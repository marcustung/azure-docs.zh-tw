---
title: 管理雙步驟驗證設定 - Azure Active Directory | Microsoft Docs
description: 管理您使用 Azure Multi-Factor Authentication 的方式，包括變更您的連絡資訊或是設定您的裝置。
services: active-directory
keywords: 多重身份验证客户端, 身份验证问题, 相关性 ID
author: eross-msft
manager: daveba
ms.reviewer: richagi
ms.assetid: d3372d9a-9ad1-4609-bdcf-2c4ca9679a3b
ms.workload: identity
ms.service: active-directory
ms.subservice: user-help
ms.topic: conceptual
ms.date: 05/23/2017
ms.author: lizross
ms.collection: M365-identity-device-management
ms.openlocfilehash: 433c2d712ca4867a5ec59f86c333511070b6d507
ms.sourcegitcommit: c6dc9abb30c75629ef88b833655c2d1e78609b89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/29/2019
ms.locfileid: "58665044"
---
# <a name="manage-your-settings-for-two-step-verification"></a>管理您的雙步驟驗證設定
本文章回答有關如何更新雙步驟驗證或 Multi-Factor Authentication 的設定。 如果您登入帳戶時遇到問題，請參閱[雙步驟驗證遇到困難](multi-factor-authentication-end-user-troubleshoot.md)以取得疑難排解說明。

## <a name="where-to-find-the-settings-page"></a>哪裡可以找到設定頁面
視您的公司使用 Azure Multi-Factor Authentication 的方式而定，有一些地方可讓您變更如電話號碼等設定。

如果您的公司支援送出特定的 URL 或步驟來管理雙步驟驗證，請遵循這些指示。 否則，下面的指示應可適用於其他所有人。 如果您遵循下列步驟，但沒有看到相同的選項，表示您的工作或學校自訂了自己的網站。 請要求系統管理員提供您的 Azure Multi-factor Authentication 入口網站連結。

**移至 [其他安全性驗證] 頁面**

- 移至 https://aka.ms/MFASetup。

    ![Proofup](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

如果按一下連結並沒有發生作用，您也可以遵循下列步驟前往 [其他安全性驗證] 頁面：

1. 登入 [https://myapps.microsoft.com](https://myapps.microsoft.com)  

2. 在右上方選取您的帳戶名稱，然後選取 [設定檔]。

3. 選取 [其他安全性驗證]。  

    ![Myapps](./media/multi-factor-authentication-end-user-manage-settings/myapps1.png)

4. [其他安全性驗證] 頁面會載入您的設定。

    ![验证](./media/multi-factor-authentication-end-user-manage-settings/proofup.png)

## <a name="i-want-to-change-my-phone-number-or-add-a-secondary-number"></a>我想要變更我的電話號碼，或新增次要號碼
請務必設定次要驗證電話號碼。  因為您的主要電話號碼和行動應用程式可能是相同的電話上，次要電話號碼會是最快的方法，您可以取回您的帳戶如果您的手機遺失或遭竊。

> [!NOTE]
> 如果您不能存取主要電話號碼，並需要協助才能進入帳戶，請參閱[雙步驟驗證遇到困難](multi-factor-authentication-end-user-troubleshoot.md)一文，以取得更多協助。  

**若要變更您的主要電話號碼︰**  

1. 在 [其他安全性驗證] 頁面上，選取包含目前電話號碼的文字方塊，並使用新的電話號碼進行編輯。  
2. 選取 [ **儲存**]。  
3. 如果此電話號碼是您針對慣用驗證選項使用的數字，必須先確定新的數字後，才能進行儲存。  

**若要新增次要電話號碼︰**  

1. 在 [其他安全性驗證] 頁面上，勾選 [替代驗證電話] 旁的核取方塊。  
2. 在文字方塊中輸入您的次要電話號碼。  
3. 選取 [儲存] 您的變更即完成。  

## <a name="require-two-step-verification-again-on-a-device-youve-marked-as-trusted"></a>需要在已標示為受信任的裝置上再次進行雙步驟驗證

根據您組織的設定，當您在您的瀏覽器上執行雙步驟驗證時，您可能會看到核取方塊指出「**X** 天內不要再問我」。 如果核取此方塊然後遺失您的裝置，或認為您的帳戶遭到入侵，您應該將雙步驟驗證還原到所有的裝置。

1. 在 [其他安全性驗證] 頁面上，選取 [還原先前受信任裝置上的 Multi-Factor Authentication]。
2. 下次您在任何裝置上登入時，程式將會提示您執行雙步驟驗證。

## <a name="how-do-i-clean-up-microsoft-authenticator-from-my-old-device-and-move-to-a-new-one"></a>如何清除舊裝置的 Microsoft驗證器並移到新裝置？
从设备上卸载该应用或重置设备时，不会删除应用在后端的激活。 如需詳細資訊，請參閱 [Microsoft Authenticator](user-help-auth-app-download-install.md)。

## <a name="next-steps"></a>後續步驟
* 取得疑難排解提示以及[使用雙步驟驗證遇到困難](multi-factor-authentication-end-user-troubleshoot.md)的說明。
* 針對不支援雙步驟驗證的任何應用程式，設定[應用程式密碼](multi-factor-authentication-end-user-app-passwords.md)。
