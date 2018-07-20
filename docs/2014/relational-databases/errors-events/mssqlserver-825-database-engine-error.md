---
title: MSSQLSERVER_825 | Microsoft Docs
ms.custom: ''
ms.date: 03/06/2017
ms.prod: sql-server-2014
ms.reviewer: ''
ms.suite: ''
ms.technology: supportability
ms.tgt_pltfrm: ''
ms.topic: conceptual
helpviewer_keywords:
- 825 (Database Engine error)
ms.assetid: f69f8214-5af1-4769-878b-117ad6eaff52
caps.latest.revision: 16
author: MashaMSFT
ms.author: mathoma
manager: craigg
ms.openlocfilehash: 315acba9b97e5f05c827c47db02bcab5a6cc9a3e
ms.sourcegitcommit: f8ce92a2f935616339965d140e00298b1f8355d7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37419041"
---
# <a name="mssqlserver825"></a>MSSQLSERVER_825
    
## <a name="details"></a>詳細  
  
|||  
|-|-|  
|製品名|SQL Server|  
|イベント ID|825|  
|イベント ソース|MSSQLSERVER|  
|コンポーネント|SQLEngine|  
|シンボル名|B_RETRYWORKED|  
|メッセージ テキスト|ファイル '%ls' のオフセット %#016I64x での読み取りは、読み取りに %d 回失敗 (エラー: %ls) した後で成功しました。 SQL Server エラー ログとシステム イベント ログ内の別のメッセージで詳細情報が報告されることもあります。 このエラー状態はデータベースの整合性を損なうので、解決する必要があります。 完全なデータベース整合性確認 (DBCC CHECKDB) を完了させてください。 このエラーには多くの要因があります。詳細については、SQL Server オンライン ブックを参照してください。|  
  
## <a name="explanation"></a>説明  
 このメッセージは、読み取り操作が少なくとも 1 回は再実行されたことを示し、ディスク ハードウェアに大きな問題があることを示しています。 このメッセージは、現時点では [!INCLUDE[ssNoVersion](../../includes/ssnoversion-md.md)] の問題を示していませんが、ディスクの問題を解決しないと、データの損失やデータベースの破損につながる可能性があります。 システム イベント ログに、問題の診断に役立つ関連するイベントが含まれている場合があります。 I/O エラーの詳細については、「[Microsoft SQL Server I/O Basics, Chapter 2](http://go.microsoft.com/fwlink/?LinkId=69370)」 (Microsoft SQL Server I/O の基礎 (第 2 章)) を参照してください。  
  
## <a name="user-action"></a>ユーザーの操作  
 次の操作を行って、原因となっている問題を特定し、解決してください。  
  
-   このメッセージのエラー ログと変数テキストを確認し、問題の原因を明らかにする手掛かりを見つける。  
  
-   ディスク システムを確認してください。 この問題は、ディスク、ディスク コントローラー、アレイ カード、またはディスク ドライバーに関連している可能性があります。  
  
-   ディスク システムの状態を調べる最新のユーティリティがあるかどうか、ディスクの製造元に問い合わせる。  
  
-   ドライバーの最新の更新があるかどうか、ディスクの製造元に問い合わせる。  
  
  