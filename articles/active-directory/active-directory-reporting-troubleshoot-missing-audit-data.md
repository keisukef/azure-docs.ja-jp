---
title: 'トラブルシューティング: Azure Active Directory アクティビティ ログにデータが見つからない | Microsoft Docs'
description: Azure Active Directory で使用可能なさまざまなレポートの一覧を示します。
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: ''
ms.assetid: 7cbe4337-bb77-4ee0-b254-3e368be06db7
ms.service: active-directory
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: identity
ms.component: compliance-reports
ms.date: 01/15/2018
ms.author: rolyon
ms.reviewer: dhanyahk
ms.openlocfilehash: 1568a7c508dbbb42143d8badf39833af90fcc376
ms.sourcegitcommit: 266fe4c2216c0420e415d733cd3abbf94994533d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/01/2018
ms.locfileid: "34589544"
---
# <a name="i-cant-find-some-actions-that-i-performed-in-the-azure-active-directory-activity-log"></a>実行した操作の一部が Azure Active Directory アクティビティ ログに見つかりません


## <a name="symptoms"></a>現象

Azure Portal でいくつかの操作を実行したので、`Activity logs > Audit Logs` ブレードでこれらの操作の監査ログが表示されるはずですが、見つかりません。

 ![レポート](./media/active-directory-reporting-troubleshoot-missing-audit-data/01.png)
 

## <a name="cause"></a>原因

操作はアクティビティの監査ログにすぐには表示されません。 ポータルの監査ログに表示されるまでには、操作を実行してから 15 分～ 1 時間かかります。

## <a name="resolution"></a>解決策

15 分～ 1 時間待ってから、操作のログが表示されるかどうかを確認します。 それでも表示されない場合は、サポート チケットを発行してください。Microsoft が調査します。


## <a name="next-steps"></a>次の手順
「[Azure Active Directory レポートに関する FAQ](active-directory-reporting-faq.md)」を参照してください。

