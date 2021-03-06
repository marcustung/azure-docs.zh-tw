---
title: 下載 Azure 帳單發票和每日使用量資料 | Microsoft Docs
description: 說明如何下載或檢視您的 Azure 帳單發票和每日使用量資料。
keywords: 帳單發票,發票下載,azure 發票,azure 使用量
services: billing
documentationcenter: ''
author: genlin
manager: adpick
editor: ''
tags: billing
ms.service: billing
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/19/2019
ms.author: banders
ms.openlocfilehash: b78fb7d697f8a72b3c2f99c4509ea6ac5c5e5566
ms.sourcegitcommit: 62d3a040280e83946d1a9548f352da83ef852085
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59281177"
---
# <a name="download-or-view-your-azure-billing-invoice-and-daily-usage-data"></a>下載或檢視您的 Azure 帳單發票和每日使用量資料

針對大多數的訂用帳戶，您可以從 [Azure 入口網站](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)下載您的發票，或透過電子郵件傳送它。 如果您是 Azure Enterprise 合約客戶 (EA 客戶)，您無法下載您的組織發票。 發票會傳送給已設定接收註冊發票的人員。

如果您是 EA 客戶，或有[Microsoft 客戶合約](#check-access-to-a-microsoft-customer-agreement)，您可以下載中的使用方式[Azure 入口網站](https://portal.azure.com/)。 其他訂用帳戶，請移至[Azure 帳戶中心](https://account.azure.com/Subscriptions)下載使用量。

只有某些角色有權取得帳單發票和使用量資訊，例如帳戶管理員或企業系統管理員。 若要深入了解取得計費資訊的存取權，請參閱[使用角色管理 Azure 計費的存取權](billing-manage-access.md)。

如果您有 Microsoft 客戶合約時，您必須是帳單設定檔擁有者、 參與者、 讀取器，或發票管理員 」 來檢視帳單及使用量資訊。 若要深入了解計費角色適用於 Microsoft 客戶合約，請參閱[帳單設定檔角色和工作](billing-understand-mca-roles.md#billing-profile-roles-and-tasks)。

[!INCLUDE [GDPR-related guidance](../../includes/gdpr-intro-sentence.md)]

## <a name="download-your-azure-invoices-pdf"></a>下載您的 Azure 發票 (.pdf)

大部分的訂用帳戶，您可以從 Azure 入口網站下載發票。 如果您有 Microsoft 客戶合約，請參閱下載發票計費的設定檔。

### <a name="download-invoices-for-an-individual-subscription"></a>下載個別的訂用帳戶的發票

1. 選取您的訂用帳戶，從[訂用帳戶頁面](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)在 Azure 入口網站中為[具有發票存取權的使用者](billing-manage-access.md)。

2. 選取 [發票]。

    ![顯示 [帳單和使用量] 選項的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/billingandusage.png)

3. 按 [下載發票] 以檢視 PDF 發票的副本。 如果它顯示**無法使用**，請參閱[為什麼我沒有看到上期的發票？](#noinvoice)

    ![顯示計費期間、下載發票和每個計費期間總費用的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/billing4.png)

4. 按一下計費期間，還可檢視每日使用量。

如需發票的詳細資訊，請參閱[了解 Microsoft Azure 帳單](billing-understand-your-bill.md)。 如需管理成本的說明，請參閱[使用 Azure 計費與成本管理避免非預期的成本](billing-getting-started.md)。

### <a name="download-invoices-for-a-microsoft-customer-agreement"></a>Microsoft 客戶合約下載發票

為每個會產生發票[帳單設定檔](billing-mca-overview.md#understand-billing-profiles)Microsoft 客戶合約。 您必須是帳單設定檔擁有者、 參與者、 讀取器或管理員從 Azure 入口網站下載發票的發票。

1. 針對 [成本管理 + 帳單] 進行搜尋。
2. 選取計費的設定檔。
3. 選取 [發票]。
4. 在發票方格中，尋找您想要下載發票的資料列。
5. 按一下省略符號 (`...`) 結尾的資料列。
6. 在下載內容功能表中，選取**發票**。

如果您沒有看到發票的上次計費期間，請參閱**更多資訊**。 <!-- Fix this -->
### <a name="noinvoice"></a>為什麼我沒有看到上期的發票？

您沒有看到發票的可能原因如下︰

- 您的 Azure 訂閱不滿 30 天。

- 尚未產生發票。 等到計費期間結束。

- 您沒有檢視發票的權限。 如果您有 Microsoft 客戶合約時，您必須是帳單設定檔擁有者、 參與者、 讀取器，或發票管理員。 針對其他訂用帳戶，您不可能會看到舊的發票如果您不是帳戶管理員。 若要深入了解取得計費資訊的存取權，請參閱[使用角色管理 Azure 計費的存取權](billing-manage-access.md)。

- 如果您不會超過您訂用帳戶的情況下，有免費試用版或每月的信用額度，您就不會取得發票，除非您有 Microsoft 客戶合約。

## <a name="get-your-invoice-in-email-pdf"></a>透過電子郵件取得發票 (.pdf)

您可以選擇加入並設定其他收件者來透過電子郵件接收您的 Azure 發票。 這項功能可能無法用於支援供應項目、Enterprise 合約或 Azure in Open 等特定訂用帳戶。 如果您有 Microsoft 的客戶合約，請參閱取得帳單設定檔將發票電子郵件中。

### <a name="get-your-subscriptions-invoices-in-email"></a>電子郵件中取得您訂用帳戶的發票

1. 在[訂用帳戶頁面](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade)中，選取您的訂用帳戶。 針對您擁有的每個訂用帳戶選擇加入。 依序按一下 [發票] 和 [以電子郵件寄送我的發票]。

    ![顯示選擇加入流程的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/InvoicesDeepLink.PNG)

2. 按一下 [選擇加入] 並接受條款。

    ![顯示加入流程步驟 2 的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep2.PNG)

3. 接受合約後，您就可以設定其他收件者。 移除收件者時，不會再儲存電子郵件地址。 如果您改變心意，則必須予以重新新增。

    ![顯示加入流程步驟 3 的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep3.PNG)

如果在依照這些步驟執行之後並未收到電子郵件，請在[設定檔的通訊喜好設定](https://account.windowsazure.com/profile)中確定您的電子郵件地址是正確的。

### <a name="opt-out-of-getting-your-subscriptions-invoices-in-email"></a>退出以電子郵件取得訂用帳戶的發票

您可以選擇不遵循的步驟上方，然後按一下 透過電子郵件取得發票**退出以電子郵件傳送發票**。 此選項會移除任何已設定為透過電子郵件接收發票的電子郵件地址。 如果您選擇使用上一步中，您可以重新設定收件者。

 ![顯示退出流程的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/InvoiceArticleStep4.PNG)

### <a name="get-your-microsoft-customer-agreement-invoices-in-email"></a>電子郵件中取得您的 Microsoft 客戶合約發票

如果您有 Microsoft 客戶合約時，您可以選擇在以電子郵件取得發票。 所有計費的設定檔擁有者、 參與者、 讀者和發票管理員會透過電子郵件取得發票。 讀取器無法更新電子郵件發票喜好設定。

1. 針對 [成本管理 + 帳單] 進行搜尋。
1. 選取計費的設定檔。
1. 在 [設定] 下方，選取 [屬性]。
1. 底下**電子郵件發票**，選取**更新電子郵件發票喜好設定**。
1. 選取 **參加**。
1. 按一下 [更新] 。

### <a name="opt-out-of-getting-your-billing-profile-invoices-in-email"></a>退出以電子郵件中取得計費的設定檔發票

您可以選擇不遵循的步驟上方，然後按一下 透過電子郵件取得發票**退出**。所有的擁有者、 參與者、 讀者和發票管理員將會退出太電子郵件取得發票。 如果您是在讀取器，您無法變更電子郵件發票喜好設定。

## <a name="download-usage"></a>下載使用量

 針對大多數的訂用帳戶，請在 [Azure 帳戶中心](https://account.azure.com/Subscriptions)內尋找您的每日使用量檔案。 如果您是 EA 客戶，或有 Microsoft 客戶合約，您可以下載使用量[Azure 入口網站](https://portal.azure.com/)。 <!-- TO DO: update PayG experience to Ibiza once it ships-->

### <a name="download-usage-from-the-account-center-csv"></a>從帳戶中心下載使用量 (.csv)

1. 以帳戶管理員身分登入 [Azure 帳戶中心](https://account.windowsazure.com/subscriptions)。

2. 選取您需要發票和使用資訊的訂用帳戶。

3. 選取 [帳單記錄]。

    ![顯示帳單記錄選項的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/Billinghisotry.png)

4. 可以查看过去六个计费周期以及当前未计费周期的对帐单。

    ![顯示計費期間、下載發票和每日使用量的選項，以及每個計費期間總費用的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/billingSum.png)

5. 選取 [檢視目前的對帳單]，以查看產生估計時的估計費用。 此資訊每天只會更新一次，可能不會包含您的所有使用量。 您的每月發票可能會與這項估計有所不同。

    ![顯示 [檢視目前對帳單] 選項的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/billingSum2.png)

    ![顯示目前費用預估的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/billingSum3.png)

6. 選取 [下載使用量]，以 CSV 檔案形式下載每日使用量資料。 如果您看到兩個可用版本，請下載第 2 個版本。

    ![顯示 [下載使用量] 選項的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/DLusage.png)

只有帳戶管理員可以存取 Azure 帳戶中心。 其他計費系統管理員 (例如擁有者) 可以使用 [計費 API](billing-usage-rate-card-overview.md)取得使用量資訊。

如需每日使用量的詳細資訊，請參閱[了解 Microsoft Azure 帳單](billing-understand-your-bill.md)。 如需管理成本的說明，請參閱[使用 Azure 計費與成本管理避免非預期的成本](billing-getting-started.md)。

### <a name="download-usage-for-ea-customers"></a>下載 EA 客戶的使用量

若要檢視及下載 EA 客戶的使用量資料，您必須是企業系統管理員，帳戶擁有者，或部門系統管理員與檢視費用啟用原則。

1. 登入 [Azure 入口網站](https://portal.azure.com)。
1. 搜尋*成本管理 + 計費*。

    ![顯示 Azure 入口網站搜尋的螢幕擷取畫面](./media/billing-download-azure-invoice-daily-usage-date/portal-cm-billing-search.png)

1. 選取 [使用量 + 費用]。
1. 針對您想要下載的月份，選取 [下載]。

### <a name="download-usage-for-your-microsoft-customer-agreement"></a>Microsoft 客戶合約下載使用量

若要檢視並下載帳單的設定檔的使用方式資料，您必須帳單設定檔擁有者、 參與者、 讀取器或發票管理員。

#### <a name="download-usage-for-billed-charges"></a>下載使用量計費的費用

1. 針對 [成本管理 + 帳單] 進行搜尋。
2. 選取計費的設定檔。
3. 選取 [發票]。
4. 在發票方格中，尋找對應至您想要下載使用量發票的資料列。
5. 按一下省略符號 (`...`) 結尾的資料列。
6. 在下載內容功能表中，選取**Azure 使用量和費用**。

#### <a name="download-usage-for-open-charges"></a>下載使用量的開放的費用

您也可以下載月-日使用量目前的計費週期，這表示有不尚未計費費用。

1. 針對 [成本管理 + 帳單] 進行搜尋。
2. 選取計費的設定檔。
3. 在 **概觀**刀鋒視窗中，按一下**下載 Azure 使用量和費用**。

## <a name="check-access-to-a-microsoft-customer-agreement"></a>檢查 Microsoft 客戶合約的存取權
[!INCLUDE [billing-check-mca](../../includes/billing-check-mca.md)]

## <a name="need-help-contact-us"></a>需要協助嗎？ 與我們連絡

如果您有任何疑問或需要協助，請[建立支援要求](https://go.microsoft.com/fwlink/?linkid=2083458)。

## <a name="next-steps"></a>後續步驟

若要深入了解您的發票和費用，請參閱：

- [了解 Microsoft Azure 帳單](billing-understand-your-bill.md)
- [了解您的 Azure 發票上的字詞](billing-understand-your-invoice.md)
- [了解的字詞，在您的 Microsoft Azure 詳細使用量](billing-understand-your-usage.md)
- [檢視您組織的 Azure 定價](billing-ea-pricing.md)

如果您有 Microsoft 的客戶合約，請參閱：

- [了解您帳單的設定檔的發票費用](billing-mca-understand-your-bill.md)
- [了解發票上的帳單設定檔中的字詞](billing-mca-understand-your-invoice.md)
- [了解您帳單的設定檔的 Azure 使用量和費用檔案](billing-mca-understand-your-usage.md)
- [檢視及下載帳單設定檔的稅務文件](billing-mca-download-tax-document.md)
- [檢視您組織的 Azure 定價](billing-ea-pricing.md)
